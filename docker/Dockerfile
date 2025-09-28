# 基础镜像
FROM dots-vllm-openai:latest

# 拷贝模型到镜像中
# COPY weights/DotsOCR /workspace/weights/DotsOCR

# 设置环境变量
ENV PYTHONPATH=/workspace/weights:$PYTHONPATH\
    MODEL_NAME=dotsocr-model \
    GPU_MEMORY_UTILIZATION=0.8 \
    MAX_MODEL_LEN=8192

# 修改 entrypoint 脚本逻辑
ENTRYPOINT ["/bin/bash", "-c"]
CMD ["set -ex; \
      echo '--- Starting setup and server ---'; \
      echo 'Modifying vllm entrypoint...'; \
      sed -i '/^from vllm\\.entrypoints\\.cli\\.main import main/a from DotsOCR import modeling_dots_ocr_vllm' $(which vllm) && \
      echo 'vllm script after patch:'; \
      grep -A 1 'from vllm.entrypoints.cli.main import main' $(which vllm) && \
      echo 'Starting server...'; \
      exec vllm serve /workspace/weights/DotsOCR \
          --tensor-parallel-size 1 \          
          --gpu-memory-utilization ${GPU_MEMORY_UTILIZATION} \
          --max-model-len ${MAX_MODEL_LEN} \
          --chat-template-content-format string \
          --served-model-name  ${MODEL_NAME} \
          --trust-remote-code"]
