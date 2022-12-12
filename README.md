# edgetpu-compiler-pad-fail

This repository goal is to reproduce a bug I found on the edgetpu compiler.

The coral edgetpucompiler fails when I put a "Pad" operation before an "Add" operation in a FPN.

In the edgetpu compiler output, it attributes the fail to the ADD operation : 
```
ADD                            1          More than one subgraph is not supported
```
but it seems that it is the presence of the just Pad that is causing this error.
The documentation says all Pads and Add operation are supported.

Moreover, it says "More than one subgraph is not supported", but with the -a option, which is activated, more than one subgraph IS supported, see the help of the edgetpu command :
```
# -a, --enable_multiple_subgraphs
#        Enable multiple(all) subgraphs (experimental flag).
```

Use the custom_retina.ipynb with "padded = True" to generate a tflite that fails compilation and "model_with_pad = False" for compilation success.

The "representative dataset is only here for providing realistic images for quantization.
I only try to make an architecture that compiles for coral here, not training a model.


For each model (padded vs. unpadded), I included : 

 - the tflite file before edgetpu compilation
 - the tflite file after edgepu compilation
 - the netron image capture before the edgetpu compilation
 - the netron image capture after the edgetpu compilation


I compile (using docker, I'm on windows) with 

```
docker run -it --rm -v ${pwd}:/home/edgetpu edgetpu_compiler edgetpu_compiler -m 13 -s -da -o model_output model_input/model.tflite > compiler.log 2>&1
```

Tensorflow version : 

'2.10.0'
