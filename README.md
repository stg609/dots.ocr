## 说明
在官方基础上稍微调整了下，1. 支持异步。2. 分离 模型推理部分（i.e. stg609/dots-ocr) 与 调用部分（stg609-dots-ocr python 包， 减少了一些无关包的依赖)

## 如何使用
1. 推理部分
1.1 执行 download_model.py 下载模型到某个目录，比如 /root/dotsocr/weights/DotsOCR
1.2 运行
docker run --name=dots-vllm-test --restart=unless-stopped -d --gpus '"device=0"' -v /root/dotsocr/weights/DotsOCR:/workspace/weights/DotsOCR -p 8000:8000 -e GPU_MEMORY_UTILIZATION=0.9  -e MAX_MODEL_LEN=17000  stg609/dots-ocr:v2025.9

2. 调用部分
pip install stg609-dots-ocr

## 如何构建
1. 推理部分
1.1 先打包 vllm.Dockerfile 镜像得到 dots-vllm-openai 镜像
1.2 然后修改 Dockerfile 中的 dots-vllm-openai 镜像信息
1.3 打包 Dockerfile

2. 调用部分
2.1 poetry build
2.2 poetry publish

## dots-ocr
请参考官网