
<p align="center">

  <h2 align="center">Animate-X: Universal Character Image Animation with Enhanced Motion Representation</h2>
  <p align="center">
    <a href=""><strong>Shuai Tan</strong></a>
    ·
    <a href="https://scholar.google.com/citations?user=BwdpTiQAAAAJ"><strong>Biao Gong</strong></a><sup>†</sup>
    ·
    <a href="https://scholar.google.com/citations?user=cQbXvkcAAAAJ"><strong>Xiang Wang</strong></a>
    ·
    <a href="https://scholar.google.com/citations?user=ZO3OQ-8AAAAJ"><strong>Shiwei Zhang</strong></a>
    <br>
    <a href="https://openreview.net/profile?id=~DanDan_Zheng1"><strong>Dandan Zheng</strong></a>
    ·
    <a href="https://scholar.google.com.hk/citations?user=S8FmqTUAAAAJ"><strong>Ruobing Zheng</strong></a>
    ·
    <a href="https://scholar.google.com/citations?user=hMDQifQAAAAJ"><strong>Kecheng Zheng</strong></a>
    ·
    <a href="https://openreview.net/profile?id=~Jingdong_Chen1"><strong>Jingdong Chen</strong></a>
    ·
    <a href="https://openreview.net/profile?id=~Ming_Yang2"><strong>Ming Yang</strong></a>            
    <br>
    <br>
        <a href="https://arxiv.org/abs/2410.10306"><img src='https://img.shields.io/badge/arXiv-Animate--X-red' alt='Paper PDF'></a>
        <a href='https://lucaria-academy.github.io/Animate-X/'><img src='https://img.shields.io/badge/Project_Page-Animate--X-blue' alt='Project Page'></a>
        <a href='https://mp.weixin.qq.com/s/vDR4kPLqnCUwfPiBNKKV9A'><img src='https://badges.aleen42.com/src/wechat.svg'></a>
        <a href='https://huggingface.co/Shuaishuai0219/Animate-X'><img src='https://img.shields.io/badge/%F0%9F%A4%97%20HuggingFace-Model-yellow'></a>
    <br>
    <b></a>Ant Group &nbsp; | &nbsp; </a>Tongyi Lab  </b>
    <br>
  </p>
</p>

This repository is the official implementation of paper "Animate-X: Universal Character Image Animation with Enhanced Motion Representation". Animate-X is a universal animation framework based on latent diffusion models for various character types (collectively named X), including anthropomorphic characters.
  <table align="center">
    <tr>
    <td>
      <img src="https://github.com/user-attachments/assets/fb2f4396-341f-4206-8d70-44d8b034f810">
    </td>
    </tr>
  </table>


## &#x1F4CC; Updates
* [2025.02.11] 🔥 Animate-X has been accepted by ICLR 2025!
* [2024.12.20] 🔥 We release our [Animate-X](https://github.com/antgroup/animate-x) inference codes.
* [2024.12.10] 🔥 We release our [Animate-X CKPT](https://huggingface.co/Shuaishuai0219/Animate-X) checkpoints.
* [2024.10.14] 🔥 Our [paper](https://arxiv.org/abs/2410.10306) is in public on arxiv.



<!-- <video controls loop src="https://cloud.video.taobao.com/vod/vs4L24EAm6IQ5zM3SbN5AyHCSqZIXwmuobrzqNztMRM.mp4" muted="false"></video> -->

## &#x1F304; Gallery
### Introduction 
<table class="center">
<tr>
    <td width=47% style="border: none">
        <video controls loop src="https://github.com/user-attachments/assets/085b70c4-cb68-4ac1-b45f-ed7f1c75bd5c" muted="false"></video>
    </td>
    <td width=53% style="border: none">
        <video controls loop src="https://github.com/user-attachments/assets/f6275c0d-fbca-43b4-b6d6-cf095723729e" muted="false"></video>
    </td>
</tr>
</table>

### Animations produced by Animate-X
<table class="center">
<tr>
    <td width=50% style="border: none">
        <video controls loop src="https://github.com/user-attachments/assets/732a3445-2054-4e7b-9c2d-9db21c39771e" muted="false"></video>
    </td>
        <td width=50% style="border: none">
        <video controls loop src="https://github.com/user-attachments/assets/f25af02c-e5be-4cab-ae64-c9e0b392643a" muted="false"></video>
    </td>
</tr>
</table>




## &#x1F680; Installation
Install with `conda`: 
```bash
conda env create -f environment.yaml
conda activate animate-x
```

When using Venv:

```sh
python -m venv venv
venv\Scripts\Activate
pip install -r requirements.txt
```



## &#x1F680; Download Checkpoints
Download Animate-X [checkpoints](https://huggingface.co/Shuaishuai0219/Animate-X) and put all files in `checkpoints` dir, which should be like:
```
./checkpoints/
|---- animate-x.pth 
|---- dw-ll_ucoco_384.onnx
|---- open_clip_pytorch_model.bin
|---- v2-1_512-ema-pruned.ckpt
└---- yolox_l.onnx
```

## &#x1F4A1; Inference 

The default inputs are a image (.jpg) and a dance video (.mp4). The default output is a 32-frame video (.mp4) with 768x512 resolution, which will be saved in `./results` dir.

1. pre-process the video.
    ```bash
    python process_data.py --source_video_paths data/videos --saved_pose_dir data/saved_pkl  --saved_pose data/saved_pose --saved_frame_dir data/saved_frames
    ```
2. run Animate-X.
    ```bash
    python inference.py --cfg configs/Animate_X_infer.yaml 
    ```


Some key parameters in the `.yaml` configuration file are described as follows. For example, users can adjust the `max_frames` or `sampling interval` of the dance video to generate videos of varying durations or speeds.
- `max_frames`: Number of frames (default as 32) in the generated video (fps: 8).
    - If you want to generage longer video with more frames, you should modify
        - `max_frames` as the number of frames
        - `seq_len` in `UNet` as the number of frames + 1
    - We take 96 frames as an example, and the config should be:
        ```python
        {
            max_frames: 96   # 1. modify `max_frames` as the number of frames (e.g. 96)
            ......
            UNet: {
                ......
                'use_sim_mask': False,
                'seq_len': 97, # 2. modify `seq_len` in `UNet` as the number of frames + 1  (e.g. 97 = 96 + 1)
            }
        }
        ```
- `round`: The number of times each test case is generated.
- `test_list_path`: The input paths for all test cases, for example:
    ```python
        [
            [2, "data/images/1.jpg", "data/saved_pose/dance_1","data/saved_frames/dance_1","data/saved_pkl/dance_1.pkl", 14],
            [2, "data/images/4.png", "data/saved_pose/dance_1","data/saved_frames/dance_1","data/saved_pkl/dance_1.pkl", 14],
            ......
        ]
    ```
    - `2` indicates that 1 frame is sampled from every 2 frames of the reference dance video to be used as input for the model.
    - `"data/images/1.jpg"` indicates the path to the reference image.
    - `"data/saved_pose/dance_1"` indicates the path to the saved pose images. (output by `process_data.py`, $I^p$, keypoints visualization)
    - `"data/saved_frames/dance_1"` indicates the path to the saved frames from the driven video. (output by `process_data.py`)
    - `"data/saved_pkl/dance_1.pkl"` indicates the path to the saved pose keypoints. (output by `process_data.py`, $p^d$, DWPose)
    - `14` indicates the random seed.
- `log_dir`: path to the generated animation videos, e.g., `./results`.


**&#10004; Some tips**:

> Although Animate-x does not rely on strict pose alignment and we did not perform any manual alignment operations for all the results in the paper, we cannot guarantee that all cases are perfect. Therefore, users can perform handmade pose alignment operations themselves, e.g, applying the overall x/y translation and scaling on the pose skeleton of each frame to align with the position of the subject in the reference image. (put in `data/saved_pose`) 


## &#x1F4E7; Acknowledgement
Our implementation is based on [UniAnimate](https://github.com/ali-vilab/UniAnimate), [MimicMotion](https://github.com/Tencent/MimicMotion), and [MusePose](https://github.com/TMElyralab/MusePose). Thanks for their remarkable contribution and released code! If we missed any open-source projects or related articles, we would like to complement the acknowledgement of this specific work immediately.

## &#x2696; License
This repository is released under the Apache-2.0 license as found in the [LICENSE](LICENSE) file.
