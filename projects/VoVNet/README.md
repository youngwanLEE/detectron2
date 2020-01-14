# [VoVNet-v2](https://github.com/youngwanLEE/CenterMask) backbone networks in Detectron2
**Efficient Backbone Network for Object Detection and Segmentation**

Youngwan Lee and Jongyoul Park

[[`CenterMask(code)`](https://github.com/youngwanLEE/CenterMask)] [[`VoVNet-v1(arxiv)`](https://arxiv.org/abs/1903.12174)] [[`VoVNet-v2(arxiv)`](https://arxiv.org/abs/1903.12174)] [[`BibTeX`](#CitingVoVNet)]


<div align="center">
  <img src="https://dl.dropbox.com/s/jgi3c5828dzcupf/osa_updated.jpg" width="700px" />
</div>

  
  
In this project, we release code for **VoVNet-v2** backbone network (introduced by [CenterMask](https://arxiv.org/abs/1903.12174)) in Detectron2.
VoVNet can  extract diverse feature representation *efficiently* by using One-Shot Aggregation (OSA) module that concatenates subsequent layers at once. Since the OSA module can capture multi-scale receptive fields, the diversifed feature maps allow object detection and segmentation to address multi-scale objects and pixels well, especially robust on small objects. VoVNet-v2 improves VoVNet-v1 by adding identity mapping that eases the optimization problem and *effective* SE (Squeeze-and-Excitation) that enhances the diversified feature representation.

## Highlight
Compared to ResNe(X)t backbone
- ***Efficient*** : Faster speed
- ***Accurate*** : Better performance, especially *small* object.


## Training

#### ImageNet Pretrained Models

We provide backbone weights pretrained on ImageNet-1k dataset.
* [VoVNetV2-39](https://dl.dropbox.com/s/q98pypf96rhtd8y/vovnet39_ese_detectron2.pth)
* [VoVNetV2-57](https://dl.dropbox.com/s/8xl0cb3jj51f45a/vovnet57_ese_detectron2.pth)
* [VoVNetV2-99](https://dl.dropbox.com/s/1mlv31coewx8trd/vovnet99_ese_detectron2.pth)


To train a model, run
```bash
python /path/to/detectron2/projects/VoVNet/train_net.py --config-file projects/VoVNet/configs/<config.yaml>
```

For example, to launch end-to-end Faster R-CNN training with VoVNetV2-39 backbone on 8 GPUs,
one should execute:
```bash
python /path/to/detectron2/projects/VoVNet/train_net.py --config-file projects/VoVNet/configs/faster_rcnn_V_39_FPN_3x.yaml --num-gpus 8
```

## Evaluation

Model evaluation can be done similarly:
```bash
python /path/to/detectron2/projects/VoVNet/train_net.py --config-file projects/VoVNet/configs/faster_rcnn_V_39_FPN_3x.yaml --eval-only MODEL.WEIGHTS <model.pth>
```


## Results on MS-COCO in Detectron2

### Note
We simply replace ResNe(X)t with VoVNetV2 in the Faster/Mask R-CNN, following the same hyper-parameters.\
We measure the inference time of all models with batch size 1 on the same V100 GPU machine.

- pytorch1.3.1
- CUDA 10.1
- cuDNN 7.3

Using this command with `--num-gpus 1`
```bash
python /path/to/detectron2/projects/VoVNet/train_net.py --config-file projects/VoVNet/configs/<config.yaml> --eval-only --num-gpus 1 MODEL.WEIGHTS <model.pth>
```

### Faster R-CNN

|Backbone|lr sched|inference time|AP|APs|APm|APl|download|
|:--------:|:---:|:--:|--|----|----|---|--------|
|R-50-FPN|3x|0.047|40.2|24.2|43.5|52.0|<a href="https://dl.fbaipublicfiles.com/detectron2/COCO-InstanceSegmentation/mask_rcnn_R_50_FPN_3x/137849600/model_final_f10217.pkl">model</a>&nbsp;\|&nbsp;<a href="https://dl.fbaipublicfiles.com/detectron2/COCO-InstanceSegmentation/mask_rcnn_R_50_FPN_3x/137849600/metrics.json">metrics</a>
|V2-39-FPN|3x|0.047|42.7|27.1|45.6|54.0|<a href="https://dl.dropbox.com/s/dkto39ececze6l4/faster_V_39_eSE_ms_3x.pth">model</a>&nbsp;\|&nbsp;<a href="https://dl.dropbox.com/s/dx9qz1dn65ccrwd/faster_V_39_eSE_ms_3x_metrics.json">metrics</a>
||
|R-101-FPN|3x|0.063|42.0|25.2|45.6|54.6|<a href="https://dl.fbaipublicfiles.com/detectron2/COCO-InstanceSegmentation/mask_rcnn_R_101_FPN_3x/138205316/model_final_a3ec72.pkl">model</a>&nbsp;\|&nbsp;<a href="https://dl.fbaipublicfiles.com/detectron2/COCO-InstanceSegmentation/mask_rcnn_R_101_FPN_3x/138205316/metrics.json">metrics</a>
|V2-57-FPN|3x|0.054|43.3|27.5|46.7|55.3|<a href="https://dl.dropbox.com/s/c7mb1mq10eo4pzk/faster_V_57_eSE_ms_3x.pth">model</a>&nbsp;\|&nbsp;<a href="https://dl.dropbox.com/s/3tsn218zzmuhyo8/faster_V_57_eSE_metrics.json">metrics</a>
||
|X-101-FPN|3x|0.120|43.0|27.2|46.1|54.9|<a href="https://dl.fbaipublicfiles.com/detectron2/COCO-InstanceSegmentation/mask_rcnn_X_101_32x8d_FPN_3x/139653917/model_final_2d9806.pkl">model</a>&nbsp;\|&nbsp;<a href="https://dl.fbaipublicfiles.com/detectron2/COCO-InstanceSegmentation/mask_rcnn_X_101_32x8d_FPN_3x/139653917/metrics.json">metrics</a>|
|V2-99-FPN|3x|0.073|44.1|28.1|47.0|56.4|<a href="https://dl.dropbox.com/s/v64mknwzfpmfcdh/faster_V_99_eSE_ms_3x.pth">model</a>&nbsp;\|&nbsp;<a href="https://dl.dropbox.com/s/zvaz9s8gvq2mhrd/faster_V_99_eSE_ms_3x_metrics.json">metrics</a>|

### Mask R-CNN

|Backbone|lr sched|inference time|box AP|box APs|box APm|box APl|mask AP|mask APs|mask APm|mask APl|download|
|:--------:|:--------:|:--:|--|----|----|---|--|----|----|---|--------|
|R-50-FPN|3x|0.055|41.0|24.9|43.9|53.3|37.2|18.6|39.5|53.3|<a href="https://dl.fbaipublicfiles.com/detectron2/COCO-InstanceSegmentation/mask_rcnn_R_50_FPN_3x/137849600/model_final_f10217.pkl">model</a>&nbsp;\|&nbsp;<a href="https://dl.fbaipublicfiles.com/detectron2/COCO-InstanceSegmentation/mask_rcnn_R_50_FPN_3x/137849600/metrics.json">metrics</a>
|V2-39-FPN|3x|0.052|43.8|27.6|47.2|55.3|39.3|21.4|41.8|54.6|<a href="https://dl.dropbox.com/s/c5o3yr6lwrb1170/mask_V_39_eSE_ms_3x.pth">model</a>&nbsp;\|&nbsp;<a href="https://dl.dropbox.com/s/21xqlv1ofn7oa1z/mask_V_39_eSE_metrics.json">metrics</a>
||
|R-101-FPN|3x|0.070|42.9|26.4|46.6|56.1|38.6|19.5|41.3|55.3|<a href="https://dl.fbaipublicfiles.com/detectron2/COCO-InstanceSegmentation/mask_rcnn_R_101_FPN_3x/138205316/model_final_a3ec72.pkl">model</a>&nbsp;\|&nbsp;<a href="https://dl.fbaipublicfiles.com/detectron2/COCO-InstanceSegmentation/mask_rcnn_R_101_FPN_3x/138205316/metrics.json">metrics</a>
|V2-57-FPN|3x|0.058|44.2|28.2|47.2|56.8|39.7|21.6|42.2|55.6|<a href="https://dl.dropbox.com/s/aturknfroupyw92/mask_V_57_eSE_ms_3x.pth">model</a>&nbsp;\|&nbsp;<a href="https://dl.dropbox.com/s/8sdek6hkepcu7na/mask_V_57_eSE_metrics.json">metrics</a>
||
|X-101-FPN|3x|0.129|44.3|27.5|47.6|56.7|39.5|20.7|42.0|56.5|<a href="https://dl.fbaipublicfiles.com/detectron2/COCO-InstanceSegmentation/mask_rcnn_X_101_32x8d_FPN_3x/139653917/model_final_2d9806.pkl">model</a>&nbsp;\|&nbsp;<a href="https://dl.fbaipublicfiles.com/detectron2/COCO-InstanceSegmentation/mask_rcnn_X_101_32x8d_FPN_3x/139653917/metrics.json">metrics</a>|
|V2-99-FPN|3x|0.076|44.9|28.5|48.1|57.7|40.3|21.7|42.8|56.6|<a href="https://dl.dropbox.com/s/qx45cnv718k4zmn/mask_V_99_eSE_ms_3x.pth">model</a>&nbsp;\|&nbsp;<a href="https://dl.dropbox.com/s/u1sav8deha47odp/mask_V_99_eSE_metrics.json">metrics</a>|


## TODO
 - [ ] Adding Lightweight models
 - [ ] Applying VoVNet for Panoptic FPN or other meta-architectures



## <a name="CitingVoVNet"></a>Citing VoVNet

If you use VoVNet, please use the following BibTeX entry.

```
@inproceedings{lee2019energy,
  title = {An Energy and GPU-Computation Efficient Backbone Network for Real-Time Object Detection},
  author = {Lee, Youngwan and Hwang, Joong-won and Lee, Sangrok and Bae, Yuseok and Park, Jongyoul},
  booktitle = {Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition Workshops},
  year = {2019}
}

@article{lee2019centermask,
  title={CenterMask: Real-Time Anchor-Free Instance Segmentation},
  author={Lee, Youngwan and Park, Jongyoul},
  journal={arXiv preprint arXiv:1911.06667},
  year={2019}
}

```
