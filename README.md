# Adversarial-patch
AIPI 590 Adversarial patch assignment

**Note** -- Instead of trying to make the patch more robust to it's location in an image, I tried to constrict the patch outputs to be close to black / white. So, my changes are not in the place_patch function, but in the patch_forward and training loop functions below.

My reason for constraining the patch as much as possible is so that I can hopefully recreat the patch by hand using embroidery thread. Since it would be basically impossible to match all of the colors generated in a non-constrained patch, black & white / greyscale would make the task more do-able.

Additionally I tried to generate patches of size 16 x 16, however they never reached any meaningful accuracy.

**Comments** Initially I tried to constrain the patch to be exactly black & white by rounding pixel values to either 0 or 1. This proved to be a problem, I believe due to the fact that the optimizer uses gradient descent, which requires smooth values to differentiate.

**Note**: I ended up making my final changes in just the training loop of the patch_attack() function. I changed the number of color channels in the patch parameter to 1 (from 3), which in theory only constrains the patches to greyscale, rather than purely black & white pixels.

I was not confident that this would be good enough for my purposes, but after generating a couple of test patches it appears that they still receive decent top 5 accuracy (up to 99% for 64 x 64). Strangely, after inspecting the pixels it appears that the 16 x 16 and 32 x 32 size patches ended up using only 2 colors. The 64 x 64 patch had some level of 'shading' between the two colors, but I was never going to be able to hand-stich a 64 x 64 patch in a reasonable amount of time so this was fine with me.
