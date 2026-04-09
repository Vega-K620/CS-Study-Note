# WSL2 + CUDA 初心者向け：ベクトル加算の実装と環境構築のハマり所
大学の1年生として、CUDAプログラミングの第一歩を踏み出しました。RTX 3080 Tiの性能を引き出すための環境構築と、ベクトル加算の実装過程を記録します。  
現在、私は競技プログラミング（AtCoderなど）を通じてアルゴリズムを学んでいますが、その知識をCUDAによる並列計算と組み合わせることで、計算効率の極限に挑戦したいと考えています。   
初めてCUDAに触れましたが、とても面白いと思います。  
大学の講義ではCUDAを扱いませんが、非常に興味深い分野なので、今後も独学で研鑽を積んでいきたいです。  

## 問題文：1000000回「1.0 + 2.0」を計算して出力します。
(とても簡単な問題ですが、CUDA初心者の僕にとってはいい始まりと思います。)
CUDAを使って快速で答えが得られます。
```cpp
/*题目背景
假设我们有两个巨大的数组 $A$ 和 $B$，每个数组都有 100 万个数字。我们要把它们对应相加，存入数组 $C$。
CPU 做法：用一个 for 循环，从 $0$ 运行到 $999,999$，一个一个加。
GPU 做法：雇佣 100 万个“小员工”（线程），每个员工只负责加其中一对数字。
大家同时动手，瞬间完成。*/
#include <iostream>
#include <cuda_runtime.h>

// CUDA 核函数：在 GPU 上运行
__global__ void vectorAdd(const float* A,const float* B,float* C,int N)
{
    // 计算当前线程处理哪个索引
    int i=blockDim.x*blockIdx.x+threadIdx.x;
    if(i<N)
    {
        C[i] = A[i] + B[i];
    }
}

int main() {
    std::cin.tie(NULL)->sync_with_stdio(false);
    int N = 1000000; // 100万个元素
    size_t size = N * sizeof(float);

    // 1. 在 CPU (Host) 上分配内存并初始化
    float *h_A = (float*)malloc(size);
    float *h_B = (float*)malloc(size);
    float *h_C = (float*)malloc(size);
    for(int i=0;i<N;i++)
    {
        h_A[i]=1.0f;
        h_B[i]=2.0f;
    }

    // 2. 在 GPU (Device) 上分配内存
    float *d_A,*d_B,*d_C;
    cudaMalloc(&d_A,size);
    cudaMalloc(&d_B,size);
    cudaMalloc(&d_C,size);

    // 3. 将数据从 CPU 拷贝到 GPU
    cudaMemcpy(d_A, h_A, size, cudaMemcpyHostToDevice);
    cudaMemcpy(d_B, h_B, size, cudaMemcpyHostToDevice);

    // 4. 启动核函数（线程配置）
    int threadsPerBlock = 256;
    int blocksPerGrid = (N + threadsPerBlock - 1) / threadsPerBlock;
    vectorAdd<<<blocksPerGrid, threadsPerBlock>>>(d_A, d_B, d_C, N);

    // 5. 将结果拷回 CPU
    cudaMemcpy(h_C, d_C, size, cudaMemcpyDeviceToHost);

    // 6. 验证结果
    bool success = true;
    for(int i=0;i<N;i++)
    {
        if(h_C[i]!=3.0f)
        {
            success=false;
            break;
        }
    }
    std::cout<<(success?"DONE! 100万个计算全部正确！":"ERROR!")<<"\n";
    for(int i=0;i<N;i++)
    std::cout<<i<<':'<<h_C[i]<<"\n";
    // 7. 释放内存
    cudaFree(d_A); cudaFree(d_B); cudaFree(d_C);
    free(h_A); free(h_B); free(h_C);
    return 0;
}
```
## ハマったポイント：  
PATHの通し方：Windows側からWSLのnvccを呼び出すと、.bashrcのPATHが反映されない問題。VS CodeをWSLモードで開くことで解決。  
アクセス権限：Linux内のファイル権限によりtasks.jsonが保存できないエラー。chownコマンドで対応。  
## 技術的な気付き：  
並列処理の考え方：forループによる逐次処理から、数百万の「スレッド」による一斉処理へのパラダイムシフト。  
メモリ管理：Host(CPU)とDevice(GPU)のメモリは物理的に分離されているため、cudaMemcpyによるデータ転送が必須。
