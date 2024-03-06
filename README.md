# Setting

- 환경: wsl2 Ubuntu 22.04.2 LTS

```python
# llmama-cpp용 venv 만들어준 후
sudo apt install cmake

git clone https://github.com/ggerganov/llama.cpp
cd llama.cpp

mkdir build
cd build
cmake .. -DLLAMA_CUBLAS=ON  # cuBLAS를 사용한 nvidia gpu 연산 가능하게 함
cmake --build . --config Release
```

# Run

## local

- ctx_size = c: context size
- n: number of tokens to predict , -1(infinity)
- ins: instruction mode
- b: batch size
- n-gpu-layers = ngl # 크게 입력해도 최대 vram 반밖에 못쓰는듯?
- t: threads

```python
# interactive 모드
./build/bin/main -m ./models/ldcc-solar-10.7b-finetuned.Q5_K_M.gguf \
  --color \
  --ctx_size 2048 \
  -n -1 \
  -ins \
	-b 256 \
  --top_k 10000 \
  --temp 0.2 \
  --repeat_penalty 1.2 \
  --n-gpu-layers 15000
  -t 16
```

![](https://github.com/jjlee6496/llama-cpp/imgs/test1.png)

gpu사용 확인

## server

```python
./build/bin/server -m ./models/ldcc-solar-10.7b-finetuned.Q5_K_M.gguf -c 1024

```

![](https://github.com/jjlee6496/llama-cpp/imgs/test2.png)

localhost 8080 포트

## llama-cpp-python

```python
export CMAKE_ARGS="-DLLAMA_CUBLAS=on"
export CUDACXX="/usr/local/cuda-11.8/bin/nvcc" # cuda 경로 넣어주기
pip install llama-cpp-python[server] --upgrade --force-reinstall --no-cache-dir
```