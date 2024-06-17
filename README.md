# YOLO9tr: Yolo9 with partial attention
This is the repo for using yolov9 with partial attention (PSA) \
This model was developed to be used in pavement damage detection based on YOLO9s Model.

## Authors

-Dr. Sompote Youwai\
AI research Group KMUTT 


<p align="center">
  <img src="https://github.com/Sompote/YOLO9tr/assets/62241733/40d64fae-23ac-46a9-a62b-5f5eb99553a0" alt="Picture11223"/>
</p>
<p align="center">
  <img src="https://github.com/Sompote/YOLO9tr/assets/62241733/851ad8f3-f92a-43af-a481-c7c83b6e6269" alt="Picture11"/>
</p>
<p align="center">
  <img src="https://github.com/Sompote/YOLO9tr/assets/62241733/902aa180-73fd-422e-985f-28a09166f52f" alt="detect_result"/>
</p>




## Deployment

To deploy this project run

```bash
git clone https://github.com/Sompote/YOLO9tr
pip install -r  requirements.txt
```


Reccomend to use weight for [YOLO9s](https://github.com/WongKinYiu/yolov9/releases/download/v0.1/yolov9-s.pt) as initial training


Train with Single GPU
 ```bash
 python train_dual.py --workers 8 --device 0 --batch 4 --data '/workspace/6400 images/data.yaml' --img 640 \
 --cfg models/detect/yolov9tr.yaml  --weights '../yolov9s' --name yolov9-tr --hyp hyp.scratch-high.yaml\
  --min-items 0 --epochs 200 --close-mosaic 15

```


Train with Dual GPU
 ```bash
 torchrun  --nproc_per_node 2 --master_port 9527 train_dual.py  \
--workers 8 --device 0,1 --sync-bn --batch 30 --data '/workspace/road damage/data.yaml'  \
--img 640 --cfg models/detect/yolov9tr.yaml --weights '../yolov9s' --name yolov9-c --hyp hyp.scratch-high.yaml \
--min-items 0 --epochs 200 --close-mosaic 15
```



Evaluation\
[YOLO9t.pt](https://drive.google.com/file/d/1DtXXICCulTPN8DP4HbVLP3T3sk5BP5HI/view?usp=share_link)
```
python val_dual.py --data data/coco.yaml --img 640 --batch 32 --conf 0.001 --iou 0.7 --device 0 --weights './yolov9tr.pt' \
--save-json --name yolov9_c_640_val
```
Inference
```
python detect_dual.py --source './data/images/horses.jpg' --img 640 --device 0 --weights './yolov9tr.pt' --name yolov9_c_640_detect
```
The file format of data can be used the same as YOLOv8 in Roboflow




