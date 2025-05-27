# Changelog  (from v7.0 and up)

## v8.0 - 27/05/25

### 🔹 New: Making Loops!

- ❗ a) Read the notes scartered on the workflow to see how to make loops. Very basically: Write in your prompt two or more prompts separated by "&&".

   • New custom node "loop-image" to support loops.
   • Added a new group "Loop segmented mask control" to see how the automatic segmented masks
   • A bunch of nodes to check and filter prompts for loops on the positive prompt and to apply it on the conditioning.


- ❗ b) Choose what image you want to proceed when using a batch on the "image chooser node"

   • Added a new group "Loops Compositing" where image is coposited. Added some preview images to tell the user if using loops and the amount of batches. Also is where you choose the image from batch.

### 🔹🔹 New:  New Text Encoder and LoRa sheduling with "Prompt Control".

- ❗ a) Schedule LoRas
   • Added "PC: Schedule LoRAs" node

- ❗ b) Schedule Prompt
   • Added "PC: Schedule Prompt (positive)"

### 🔹🔹🔹 Bug fix on outpainting: 

- 1.   Because of the AE refactoring to set/get I need to mute the second composite instead of bypass since "Get" original image gets it no matter if it is bypassed or not, so it always delivers the original to the switch node, thus, saving the original image instead of the oupainted one. Muting the group fixes it.



## v7.....

### Added or Changed
