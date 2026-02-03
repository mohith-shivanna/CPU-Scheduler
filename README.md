# CPU-Scheduler
A scalable workload scheduler that intelligently distributes computational tasks across multiple CPU cores. Demonstrates scheduling strategies with example kernels (matrix multiplication, fused multiply-add) and includes a web-based interface for result visualization and performance benchmarking.

## Features

- **Multi-core Task Distribution** - Efficiently schedules and distributes workloads across available CPU cores
- **Built-in Example Applications**:
  - Fused Multiply-Add (simple floating-point operations)
  - Matrix Multiplication (optimized with loop unrolling and memory bandwidth techniques)
- **Performance Profiling** - Detailed benchmarking and timing analysis
- **Web Interface** - Interactive dashboard for visualizing results and managing experiments
- **CSV Export** - Export results for further analysis

## Quick Start

### Prerequisites
- CMake 3.10+
- C++11 compatible compiler (GCC, Clang, or MSVC)
- Python 3.8+ (for web interface)

### Build

```bash
cmake -S . -B build-macos -DCPU_ONLY=ON
cmake --build build-macos -j

RUN EXAMPLE

# Fused Multiply-Add
./build-macos/sched 65536 8 2 MultiplyAdd 256 1 1

# Matrix Multiplication
./build-macos/sched 256 4 2 MatrixMultiply 16 1 1

USAGE

./sched <meanVectorSize> <batchSize> <maxCores> <kernelName> <threadsPerBlock> <maxCoresPerKernel> <verbose>
Parameters:

meanVectorSize - Size of vectors/matrices
batchSize - Number of tasks in batch
maxCores - Maximum cores to utilize
kernelName - Kernel to run (MultiplyAdd or MatrixMultiply)
threadsPerBlock - Threads per processing block
maxCoresPerKernel - Max cores per kernel instance
verbose - Verbose output (0/1)


Web Interface

Start the Flask web server:
cd webapp
python app.py


PROJECT STRUCTURE


├── main.cpp                          # Entry point and CLI argument handling
├── scheduler.cu / scheduler.cuh      # Core scheduler implementation
├── multiplyAdd.cu / multiplyAdd.cuh  # Fused Multiply-Add kernel
├── matrixmultiply.cu / matrixmultiply.cuh # Matrix Multiplication kernel
├── cpu/                              # CPU-only implementations
├── webapp/                           # Flask web interface
│   ├── app.py                        # Flask application
│   ├── templates/                    # HTML templates
│   └── static/                       # CSS/JS assets
└── build-macos/                      # Build output directory



Adding Custom Applications

The scheduler can be extended with custom kernels:

Create kernel files - mykernel.cu and mykernel.cuh
Define classes:
MyKernel - Core kernel implementation
BatchMyKernel - Batch operation handler
Implement required methods:
InitializeData() - Initialize input data
AcquireDeviceResources() - Reserve resources
ReleaseDeviceResources() - Free resources
FinishHostExecution() - Finalize and record results
Register in main.cpp - Add batch creation and experiment execution


Output
Results are saved as CSV files in results:

MatrixMultiplyKernelResults.csv - Per-kernel timing data
MatrixMultiplyBatchResults.csv - Aggregate batch results
Similar files for MultiplyAdd


Contributing

Contributions are welcome! Please fork the repository and submit a pull request.

References

Based on the Multi-GPU Scheduler framework (adapted for CPU-only execution)
