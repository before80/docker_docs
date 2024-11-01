+++
title = "GPU 支持"
date = 2024-10-23T14:54:40+08:00
weight = 80
type = "docs"
description = ""
isCJKLanguage = true
draft = false

+++

> 原文：[https://docs.docker.com/desktop/gpu/](https://docs.docker.com/desktop/gpu/)
>
> 收录该文档的时间：`2024-10-23T14:54:40+08:00`

# GPU support in Docker Desktop - Docker Desktop 中的 GPU 支持

> **Note**
>
> 
>
> Currently GPU support in Docker Desktop is only available on Windows with the WSL2 backend.
>
> ​	目前，Docker Desktop 中的 GPU 支持仅适用于使用 WSL2 后端的 Windows。

## 在 WSL2 中使用 NVIDIA GPU - Using NVIDIA GPUs with WSL2

Docker Desktop for Windows supports WSL 2 GPU Paravirtualization (GPU-PV) on NVIDIA GPUs. To enable WSL 2 GPU Paravirtualization, you need:

​	Windows 版 Docker Desktop 支持 NVIDIA GPU 的 WSL 2 GPU 虚拟化（GPU-PV）。要启用 WSL 2 GPU 虚拟化，您需要：

- A machine with an NVIDIA GPU
  - 一台配有 NVIDIA GPU 的计算机

- Up to date Windows 10 or Windows 11 installation
  - 最新的 Windows 10 或 Windows 11 系统

- [Up to date drivers](https://developer.nvidia.com/cuda/wsl) from NVIDIA supporting WSL 2 GPU Paravirtualization
  - 支持 WSL 2 GPU 虚拟化的 NVIDIA [最新驱动程序](https://developer.nvidia.com/cuda/wsl)

- The latest version of the WSL 2 Linux kernel. Use `wsl --update` on the command line
  - 最新版本的 WSL 2 Linux 内核。在命令行中使用 `wsl --update` 更新

- To make sure the [WSL 2 backend is turned on](https://docs.docker.com/desktop/wsl/#turn-on-docker-desktop-wsl-2) in Docker Desktop
  - 确保 Docker Desktop 中已启用 [WSL 2 后端](https://docs.docker.com/desktop/wsl/#turn-on-docker-desktop-wsl-2)


To validate that everything works as expected, execute a `docker run` command with the `--gpus=all` flag. For example, the following will run a short benchmark on your GPU:

​	要验证一切是否正常工作，可以执行带有 `--gpus=all` 标志的 `docker run` 命令。例如，以下命令将在您的 GPU 上运行一个简短的基准测试：

```console
$ docker run --rm -it --gpus=all nvcr.io/nvidia/k8s/cuda-sample:nbody nbody -gpu -benchmark
```

The output will be similar to:

​	输出示例如下：

```console
Run "nbody -benchmark [-numbodies=<numBodies>]" to measure performance.
        -fullscreen       (run n-body simulation in fullscreen mode)
        -fp64             (use double precision floating point values for simulation)
        -hostmem          (stores simulation data in host memory)
        -benchmark        (run benchmark to measure performance)
        -numbodies=N    (number of bodies (>= 1) to run in simulation)
        -device=<d>       (where d=0,1,2.... for the CUDA device to use)
        -numdevices=<i>   (where i=(number of CUDA devices > 0) to use for simulation)
        -compare          (compares simulation results running once on the default GPU and once on the CPU)
        -cpu              (run n-body simulation on the CPU)
        -tipsy=<file.bin> (load a tipsy model file for simulation)

> NOTE: The CUDA Samples are not meant for performance measurements. Results may vary when GPU Boost is enabled.
	注意：CUDA 示例并不适用于性能测量。启用 GPU Boost 时结果可能有所不同。

> Windowed mode 窗口模式
> Simulation data stored in video memory 模拟数据存储在视频内存中
> Single precision floating point simulation 单精度浮点模拟
> 1 Devices used for simulation  使用 1 个设备进行模拟
MapSMtoCores for SM 7.5 is undefined.  Default to use 64 Cores/SM
GPU Device 0: "GeForce RTX 2060 with Max-Q Design" with compute capability 7.5

> Compute 7.5 CUDA device: [GeForce RTX 2060 with Max-Q Design]
30720 bodies, total time for 10 iterations: 69.280 ms
= 136.219 billion interactions per second
= 2724.379 single-precision GFLOP/s at 20 flops per interaction
```

Or if you wanted to try something more useful you could use the official [Ollama image](https://hub.docker.com/r/ollama/ollama) to run the Llama2 large language model.

​	如果您想尝试一些更有用的示例，可以使用官方的 [Ollama 镜像](https://hub.docker.com/r/ollama/ollama) 运行 Llama2 大型语言模型。

```console
$ docker run --gpus=all -d -v ollama:/root/.ollama -p 11434:11434 --name ollama ollama/ollama
$ docker exec -it ollama ollama run llama2
```
