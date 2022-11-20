My color recognition program tells the user the color of a picture using image classification.

I created this project on the Jetson nano, using a retrained resnet18 model, containing my dataset of colors. It uses imagenet.py to run the recognition 
code on the inputted image.

1. To get started, the first thing you need to do is have your Jetson Nano plugged in with the SD card inside. 
2. Then, you need to find a dataset, in my case, I was working with colors, but you can with whatever you work with. 
3. Then you will need to create a test, train, and val folder if your dataset doesn't come 
with it.(I had to)
4. Now, you need create a labels.txt document inside jetson-inference/python/training/classification/data/colors, inside all it will have is the names of
what you want your code to be recognized as, in my case it's just all my colors.
5. Then you can retrain your dataset by first going into the docker and changing directories into jetson-inference/python/training/classification. 
6. Then you need to run this code to actually retrain it: python3 train.py --model-dir=models/colors data/colors, of course with your dataset name in place 
of colors.(This will take awhile). 
7. Next, you need to export this network by running: python3 onnx_export.py --model-dir=models/colors in the same jetson-inference/
python/training/classification within the docker. After this completes if you go to jetson-inference/python/training/classification/models/colors, there
should be a new resnet18.onnx file, that is your retrained model!
8. Next you need to set the variables for NET and DATASET: NET=models/colors  DATASET=data/colors
9. After this, all you need to do is run the code. You can do this by running this command: sudo imagenet.py --model=python/training/classification/$NET/
resnet18.onnx --input_blob=input_0 --output_blob=output_0 --labels=python/training/classification/$DATASET/labels.txt $DATASET/
training_dataset/black/black17.png output_colors/. 
NOTES FOR THE LAST CODE: 
I had to put sudo in front because for some reason it said I didn't have permission, so you don't need that unless you run into the same issue.(Sudo 
basically just overrides the restrictions)
The "--model=python/training/classification/$NET/resnet18.onnx" is to call the new retrained dataset. All the code in front is a path to lead to it.
The "--labels=python/training/classification/$DATASET/labels.txt" is for calling the labels.txt doc we made just so the code knows what to call the image
you just inputted.
The "$DATASET/training_dataset/black/black17.png" is to find what image you want to input. This would be within your dataset. 
The "output_colors/" part is simply telling the code where to ouput the result to. 
10. If you want to see the code on your host computer, you can scp the result to your desktop by running this command for Mac: scp <nanousername>@192.168.
55.1:/home/<nanousername>/jetson-inference/output_colors/0.jpg ./  (My output_colors folder is in jetson-inference, it might be different for you, check to
make sure the path to your output_colors folder is correct.)

And that's it!

Project Demo vid: 
