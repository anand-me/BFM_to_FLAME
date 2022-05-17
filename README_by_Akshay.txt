FLAMEModel and VOCA_v1 are two distinct directories || However, further FLAME would act as a prerequisite for VOCA_v1  ~~~~>  ||| Please read the last-paragraph carefully ||| <~~~~
As a first step, it's better to check if FLAME works fine or need some debugging
or,
We can bypass FLAME by directly going to VOCA_v1 and set-up/activate the virtual environment there

Inside FLAME directory there is a sub-directory called BFM_to_FLAME
BFM_to_FLAME consits of model, data an all the necessary files needed for FLAME model to run

If you choose to run FLAME separately,

FLAME is writeen in tensorflow (version 1.15.0) and python it mainly requires --> boost package (C++ package) for a python library called "Mesh"
This enables the user to manipulate and visualize the mesh on there geometry, further mesh rendering and so on ...

How to run FLAME?

The best way to do so is create a virtual environment and store all the packages of TF and python so in future we don't have any problem re-running the model.

To create a virtual environment for FLAME model:

cd /gpfs/research/mecfd/Akshay/CompVisionFD/FLAMEModal/
yo will see two folders here: BFM_to_FLAME --> contains all the codes/data/model
			      FLAMEModalVenv --> this is the virtual environment for FLAMEModalVenv
			      it contains all the packages for python; 
			      
activate virtual environment: source FLAMEModalVenv/bin/activate

after activating the virtual environment: run the code: python mesh_convert.py 
output of FLAME --> the output file is saved in /BFM_to_FLAME/output/same_name_as_input.obj
This is BFM face shape converted to FLAME shape

~~~~~~~~~~~~
Background:
~~~~~~~~~~~~
FLAME (Faces Learned with an Articulated Model and Expressions) is designed to work with existing graphics software and be easy to fit to data. It uses a linear shape space trained from 3800 scans of human heads. FLAME combines this linear shape space with an articulated jaw, neck, and eyeballs, pose-dependent corrective blendshapes, and additional global expression

FLAME and VOCA are two distinct models, however, the VOCA has a pre-requisite of trained FLAME (tuned) faces. We can run FLAME individually OR run it directly from VOCA directory itself (I've saved FLAME model and data in VOCA). The steps are mentioned below:-
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
 VOCA_v1
~~~~~~~~~~~~~~~~~~
Background
~~~~~~~~~~~~~~~~~~
VOCA (Voice Operated Character Animation) takes any speech signal as input - even speech in languages other than English and realistically animates a wide range of adult faces. Conditioning on subject labels during training allows the model to learn a variety of realistic speaking styles. VOCA also provides animator controls to alter speaking style, identity-dependent facial shape, and pose (i.e. head, jaw, and eyeball rotations) during animation.

## Step-by-step guide to run VOCA on RCC

step1: 		cd /gpfs/research/mecfd/Akshay/CompVisionFD/VOCA_v1/
step2: 		module load purge
step3: 		module load gnu openmpi cuda
step4: 		source /gpfs/research/mecfd/Akshay/CompVisionFD/VOCA_v1/NAME_OF_VIRTUAL_ENV_ NewVen
step5: 		pip install Mesh
step6: 		cd /path/to/your/mesh/installation 
step7: 		pip install --upgrade pip setuptools wheel
step8: 		pip install opencv-python
step9: 		cd /voca #(the directory where the codes .py files are located)
step10: 	pip install -r requirements.txt
step11: 	BOOST_INSTALL_DIRS=/opt/rcc/gnu/include/boost make all
step 12: 	python-c "import mesh"


~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
# As virtual environment is already created so we can skip all the above mentioned steps

step1:  cd /gpfs/research/mecfd/Akshay/CompVisionFD/VOCA_v1/
step2:  source /gpfs/research/mecfd/Akshay/CompVisionFD/VOCA_v1/NAME_OF_VIRTUAL_ENV_ NewVen
If we want to run FLAME from here
step3: cd /BFM_to_FLAME/
step4: python mesh_aa_convert.py #this file reads the BFM faces (x y z vertices of BFM) and generates the FLAME articulated faces (x y z of BFM translated to FLAME)

step5: if we want to run the VOCA model, cd /voca
step6: python run_voca.py --tf_model_fname './model/gstep_52280.model' --ds_fname './ds_graph/output_graph.pb' --audio_fname './audio/test_sentence.wav' --template_fname './template/FLAME_sample.ply' --condition_idx 3 --out_path './animation_output'
OUTPUT: above line results the animation meshes given audio sequences, and renders the animation sequence to a video.

step7: python run_voca.py --tf_model_fname './model/gstep_52280.model' --ds_fname './ds_graph/output_graph.pb' --audio_fname './audio/test_sentence.wav' --template_fname './template/FLAME_sample.ply' --condition_idx 3 --uv_template_fname './template/texture_mesh.obj' --texture_img_fname './template/texture_mesh.png' --out_path './animation_output_textured'

Output: The above line helps to visualize the meshes with a pre-defined texture (obtained by fitting FLAME to an image )
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
