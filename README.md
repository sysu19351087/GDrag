## Installation

```
conda env create -f environment.yaml
```

### 安装SAM:

下载SAM的预训练文件(https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth)，将该文件放至./segment-anything-main/ckpts文件夹下。

通过以下方式在dragdiff环境中安装segment_anything模块包：

```
conda activate dragdiff
cd ./segment-anything-main
pip install -e .
```



## Gradio Demo

```
python drag_ui.py
```

### Relocation:

![relocation_gui](./relocation_gui.png)

(1) 输入图片并进行LoRA微调；

(2) 借助SAM模型，输入Segment Prompt(如Positive point，Negative point，Box)进行编辑区域的分割；

(3) 指定drag的起点与终点位置；

(4) 执行drag编辑。



### Rotation:

![rotation_gui](./rotation_gui.png)

(1)(2)(5)同上；

(3) 自定义三维椭圆的大小以及初始位置，包括其中心点的xy坐标，椭圆x轴和y轴的长度，以及绕y轴或z轴的初始旋转角度；

(4) 指定Drag点，指定旋转轴的三维向量(方向)，旋转角度，以及旋转轴的二维坐标，然后进行椭圆的旋转。



### Rescaling:

![rescaling_gui](./rescaling_gui.png)

(1)(2)(4)同上；

(3) 选择缩放类型为整体(Entirety)或局部(Part)，如果是整体，需要指定缩放中心和缩放比例；如果是局部，需要在图中指定drag的起点和终点位置。



## Evaluation

DragBench数据集原始数据位于./drag_bench_evaluation/drag_bench_data，此外，针对我们的方法，需要先对该数据集进行一些预处理，预处理数据位于./drag_bench_evaluation/drag_bench_data_preprocess_result_better。以下是模型评估流程：

### (1) 分别针对每个样本训练LoRA:

```
cd ./drag_bench_evaluation
python run_lora_training.py
```

生成的所有LoRA文件将位于./drag_bench_evaluation/drag_bench_lora。

### (2) 利用GDrag完成对每个样本的Drag编辑:

```
cd ./drag_bench_evaluation
python run_drag_diffusion_GDrag.py
```

### (3) 评估生成结果的质量:

```
cd ./drag_bench_evaluation
python run_eval_point_matching.py --eval_root results_dir      # 评估Distance指标
python run_eval_similarity.py --eval_root results_dir          # 评估Lpips指标
```

