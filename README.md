# triton-with-docker-bridge
A tutorial for NVIDIA Triton Inference Server and using docker bridge.

* Prepare
    ```bash
    ./fetch_models.sh
    ```
* Create bridge: `dev-net`
    ```bash
    docker network create --driver bridge dev-net
    ```
* Triton Server
    ```
    docker run --gpus=1 -it --rm --name server \
    --net="dev-net" \
    -v ${PWD}/model_repository:/models \
    nvcr.io/nvidia/tritonserver:23.11-py3 \
    tritonserver --model-repository=/models
    ```
* Client
    ```bash
    docker run -it --rm --name client \
    --net="dev-net" \
    -v ./cat.jpg:/workspace/cat.jpg \
    nvcr.io/nvidia/tritonserver:23.11-py3-sdk \
    /workspace/install/bin/image_client \
    -u server:8001 -i gRPC \
    -m densenet_onnx -c 3 -s INCEPTION /workspace/images/mug.jpg
    ```
* Client
    ```bash
    docker run -it --rm --name client \
    --net="dev-net" \
    nvcr.io/nvidia/tritonserver:23.11-py3-sdk \
    bash
    ```
    ```bash
    /workspace/install/bin/image_client \
    -u server:8001 \
    -i gRPC \
    -m densenet_onnx -c 3 -s INCEPTION /workspace/images/mug.jpg
    ```

# Reference
https://github.com/triton-inference-server/server/blob/main/docs/customization_guide/deploy.md

# Related Tools
* Install ifconfig
  ```
  apt install net-tools
  ```
* Install ping
  ```
  apt install iputils-ping
  ```
