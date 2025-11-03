# Adversarial-patch
AIPI 590 Adversarial patch assignment

**All uses of gemini cited in comments** 
(Only in show_patch function)

**Code explanation** Instead of trying to make the patch more robust to it's location in an image, I tried to constrict the patch outputs to be close to black / white. So, my changes are not in the place_patch function, but in the patch_forward and training loop functions below.

My reason for constraining the patch as much as possible is so that I can hopefully recreate the patch by hand using embroidery thread. (Update: I started this, but greatly underestimated the time commitment). Since it would be basically impossible to match all of the colors generated in a non-constrained patch, black & white / greyscale would make the task more do-able.

Additionally I tried to generate patches of size 16 x 16, however they never reached any meaningful accuracy.

Initially I tried to constrain the patch to be exactly black & white by rounding pixel values to either 0 or 1. This proved to be a problem, I believe due to the fact that the optimizer uses gradient descent, which requires smooth values to differentiate.

I ended up making my final changes in just the training loop of the patch_attack() function. I changed the number of color channels in the patch parameter to 1 (from 3), which in theory only constrains the patches to greyscale, rather than purely black & white pixels.

I was not confident that this would be good enough for my purposes, but after generating a couple of test patches it appears that they still receive decent top 5 accuracy (up to 99% for 64 x 64). Strangely, after inspecting the pixels it appears that the 16 x 16 and 32 x 32 size patches ended up using only 2 colors. The 64 x 64 patch had some level of 'shading' between the two colors, but I was never going to be able to hand-stich a 64 x 64 patch in a reasonable amount of time so this was fine with me.

**Note** Since I commented out some of the classes (including goldfish and school bus) some of the sample code at the end of the notebook didn't run to perform patch attacks with these classes. I would have re-run the notebook to get everything to output clean, but I ran out of colab compute after debugging / generating patches. 

There was also something weird with the validation results. During training they returned very high values, but in the tables from the example code the results were much worse. Since I got my adversarial patch to work using the test online, I'm assuming the higher values are accurate, and pasted them here. 

Validation results:
Validation results for bow tie and 16: {'acc': 0.0, 'top5': 0.006000000052154064}

Validation results for bow tie and 32: {'acc': 0.13527053594589233, 'top5': 0.3336673378944397}

Validation results for bow tie and 64: {'acc': 0.9884999990463257, 'top5': 0.9994999766349792}

Validation results for coffee mug and 16: {'acc': 0.0, 'top5': 0.0030060119461268187}

Validation results for coffee mug and 32: {'acc': 0.30611222982406616, 'top5': 0.6307615041732788}

Validation results for coffee mug and 64: {'acc': 0.8855000138282776, 'top5': 0.9934999942779541}

Validation results for safe and 16: {'acc': 0.0075150299817323685, 'top5': 0.044088177382946014}

Validation results for safe and 32: {'acc': 0.018537074327468872, 'top5': 0.2394789606332779}

Validation results for safe and 64: {'acc': 0.8289999961853027, 'top5': 0.9585000276565552}

