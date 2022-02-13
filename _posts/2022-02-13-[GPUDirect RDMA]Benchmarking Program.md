---
tags: GPUDirect_RDMA RDMA
toc: True
title: "[GPUDirect RDMA]Benchmarking Program"
---

## Benchmarking tools

### GTC

#### 1.Benchmarking GPUDirect RDMA on Modern Server Platforms (2014)

> ref: <https://developer.nvidia.com/blog/benchmarking-gpudirect-rdma-on-modern-server-platforms/>


* As we are interested in the low-level performance of our platforms, we used three IB Verbs-level benchmark programs. For latency we used ibv_ud_pingpong from libibverbs-1.1.7, while for bandwidth we adopted ib_rdma_bw from perftest-1.3.0. The former uses the IBV_WR_SEND primitive and the UD protocol; the latter the IBV_WR_RDMA_WRITE primitive and the RC protocol. Those test programs have been properly modified both to target GPU memory and take advantage of GPUDirect RDMA. We also employed a modified version of ib_write_bw, out of perfest-2.0, to exploit the full peak performance of the PCIe x16 Gen3 link on the Connect-IB HCA. The end of this post gives the details of the hardware platforms and the software we used.

##### 2. Accelerating IO in the Modern Data Center: Network IO

> ref: <https://developer.nvidia.com/blog/accelerating-io-in-the-modern-data-center-network-io/>

* MPI over
