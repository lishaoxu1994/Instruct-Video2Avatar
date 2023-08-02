# Instruct-Video2Avatar: Video-to-Avatar Generation with Instructions [[Arxiv Paper](https://arxiv.org/abs/2306.02903)]

"Make him look like Vincent Van Gogh"
<p float="left">
  <img src="demo/demo1_1.gif" width="400" />
  <img src="demo/demo1_2.gif" width="400" /> 
</p>
"He should be in "Zelda: Breath of the Wild""

<p float="left">
  <img src="demo/demo2_1.gif" width="400" />
  <img src="demo/demo2_2.gif" width="400" /> 
</p>

"He should look 100 years old"
<p float="left">
  <img src="demo/demo3_1.gif" width="400" />
  <img src="demo/demo3_2.gif" width="400" /> 
</p>


#### Usage example for image editing:
Installation
```shell
git clone https://github.com/timothybrooks/instruct-pix2pix
cd instruct-pix2pix
conda env create -f environment.yaml
```
Usage
```shell
conda activate ip2p
bash scripts/download_checkpoints.sh
python edit_cli.py --steps 100 --resolution 512 --seed 1371 --cfg-text 4.5 --cfg-image 1.2 --input imgs/example.jpg --output imgs/output.jpg --edit "turn him into a cyborg"
```

#### Example-based Style transfer:
For Example-based Image Synthesis, we recommend the [Ebsynth.exe](https://ebsynth.com/) for windows. `select keyframes->select video->run all.` For our task, the keyframes is the edited portrait image, and the video images are the training or rendering images.


#### Usage example for video2avatar of INSTA:



Installation
```shell
git clone --recursive https://github.com/Zielon/INSTA.git
cd INSTA
cmake . -B build
cmake --build build --config RelWithDebInfo -j
cd INSTANT
```
After building the project you can either start training an avatar from scratch or load a snapshot. For training, we recommend a graphics card higher or equal to `RTX3090 24GB` and `32 GB` of RAM memory. Training on a different hardware probably requires adjusting options in the config:
```shell
	"parent": "main.json",
	"max_steps": 30000,
	"max_cached_bvh": 4000,
	"max_images_gpu": 1700,
	"use_dataset_cache": true,
	"render_novel_trajectory": false,
	"render_from_snapshot": true
```


Usage
```shell
cd INSTANT
## Training
./build/rta --config insta.json --scene data/obama --height 512 --width 512 --no-gui
## Loading from a checkpoint
./build/rta --config insta.json --scene data/obama --height 512 --width 512 --no-gui --snapshot data/obama/experiments/insta/debug/snapshot.msgpack
```
For training, set `"render_from_snapshot": false`. For rendering from a checkpoint, set `"render_from_snapshot": true`. For rendering novel views, set `"render_novel_trajectory": true`.

#### Instructions for our pipeline:
1) Select one keyframe and execute image editing with InstructPix2Pix.

2) Update the images of dataset with ebsynth, with original/rendering images and the edited keyframe.

3) Train avatar and render portrait images, continue to step 2.

&emsp;&emsp;Tips: The open mouth keyframe is better. Three iterations of step 2&3 is sufficient.

### Dataset
We are releasing part of our training dataset and the checkpoint. We use the [avatars](https://keeper.mpdl.mpg.de/d/5ea4d2c300e9444a8b0b/) from [INSTA](https://github.com/Zielon/INSTA). 

Some checkpoints [Edited Avatars](https://1drv.ms/f/s!AopReTZiOgmocrqOmYNb-5iFV8s?e=833Dcl).

For Dataset Generation of original videos, we direct the user to [INSTA](https://github.com/Zielon/INSTA) and [Metrical Photometric Tracker](https://github.com/Zielon/metrical-tracker)


## BibTeX

```
@misc{li2023instructvideo2avatar,
      title={Instruct-Video2Avatar: Video-to-Avatar Generation with Instructions}, 
      author={Shaoxu Li},
      year={2023},
      eprint={2306.02903},
      archivePrefix={arXiv},
      primaryClass={cs.CV}
}
```
## Acknowledgment
InstructPix2Pix: Learning to Follow Image Editing Instructions. (https://github.com/timothybrooks/instruct-pix2pix)


ebsynth: Fast Example-based Image Synthesis and Style Transfer. (https://github.com/jamriska/ebsynth)

INSTA - Instant Volumetric Head Avatars. 
(https://github.com/Zielon/INSTA/tree/master)


