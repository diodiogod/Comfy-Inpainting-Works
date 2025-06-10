# Changelog (from v7.0 and up)

---

## \[v8.12]
### 🐞 Bug Prevention

1. Remove dependencies (no longer needed) cutom_nodes:
   * [Crystools](https://github.com/crystian/ComfyUI-Crystools) > changes all swithc nodes to index switch
   * [Logic](https://github.com/theUpsider/ComfyUI-Logic) > I had missed one Float node on Control-Panel

2. Resize Mask (context mask) will error out, node #1834, on v8.11 I have changed mask resize to use lanczos, but it is bugged, so I changed that to other method like bicube. let's wait for the dev to fix it

---

## \[v8.1] - 06/06/25

### 🐞 Bug Prevention

1. ❗ Outpainting will be automatically disabled, even if remains enabled on the control panel, when using Loops. This occurs because loops pass back the image during each iteration, expanding/outpainting it on top of the previous one. This process results in a mismatch in the final composite.

### ➕ New

* 🔹 COMPACT updated to v8.1 (loops etc)

---

## \[v8.0] - 27/05/25

### ➕ New

* 🔹 Making Loops!

  * ❗ Read the notes scattered on the workflow to see how to make loops. Basic usage: separate prompts with `&&`.
  * • New custom node `loop-image` to support loops.
  * • Added group “Loop segmented mask control” to preview segmented masks.
  * • Nodes to check/filter loops in the positive prompt and apply conditioning.
* 🔹 Batch Image Chooser

  * ❗ Choose which image to process in a batch via the “image chooser” node.
  * • Added group “Loops Compositing” for batch compositing with preview images.
* 🔹🔹 Prompt Control Enhancements

  * • Added text encoder and LoRa scheduling:

    * `PC: Schedule LoRAs` node
    * `PC: Schedule Prompt (positive)` node

### 🐞 Bug Fixes

* 🔹🔹🔹 Outpainting: Because of the AE refactoring to set/get I need to mute the second composite instead of bypass since "Get" original image gets it no matter if it is bypassed or not, so it always delivers the original to the switch node, thus, saving the original image instead of the oupainted one. Muting the group fixes it.

---

## \[v7.3] - 12/05/25

### 🐞 Bug Fixes

1. ❗ The External Loaded mask and Context Mask were mismatched when the image was resized. The workflow will now resize both masks before cropping (to ensure they are divisible by 8). when resized. Now both masks resize before cropping (divisible by 8).

---

## \[v7.2] - 06/05/25

### ➕ New

1. ❗ Option to invert aura direction. Useful when inpainting the whole background leaving a small area to not change, in this cases the aura grows OUT of the not painted area, not inside it. Another way to put it: a black aura grows inside the inpainted area when this is ON.
Use case: if you want to inpaint the BG and leave the charcater, but want the Ksampler to understand the boundaries of said character, using a very fainted "black" aura makes the ksampler to keep the surrounding area in the beggining while slowly painting it over the denoising process since it is a gray area. (turn on ksampler preview so you can see this happening). 
Recommended settings: Low visibility (30) with a small grow (5).  


### 🔄 Refactoring

2. ❗ Anything Everywhere (AE) refactor:

   * Switched AE to Set/Get across multiple groups:

     * for Choice: “Auto Preprocessor for Canny OR Depth”
     * “control room”
     * “Conditioning Index Switch” after ControlNet
     * “Get Unet Checkpoint Name for Metadata”
     * Alimama ControlNet Hash “Switch (Any)” on save group
   * LoRA Stacker:

     * 💊 CR Apply LoRA Stack (direct connect)
     * LoRA Stack to String Converter on "Get LoRAs names..." group
   * for ALIMAMA_ControlNet on "ControlNetInpaintingAliMamaApply"

   * for VAE 
     * on ControlNetInpaintingAliMamaApply and InpaintModelCon 

   * FOR Dev FILL
     * on "InstructPixToPixConditioning"
     * on VAE encode and 2 decodes

   * For CLIP 
     * on CLIP Text Encode (Prompt pos and neg)

   * For Model 
     * on Detailer Daemon Samples and Normal Ksampler

   * For EvalNeg
     * on "Using negative? switch"
     * on control room group
     * on "Inpaint with Detail Daemon?" group

   * For PositiveP 
     * on "Text Concatenate" for metadata

   * For Guidance for Flux Guidance

   * For Sampler and Sheduler 
     * on samplers and on save node

   * For Steps, Seed, CFG, Denoise...

### 🐞 Bug Fixes

3. Fixed: "Resize Mask" to "CropMask". Resize would mismatch the mask but would stretch it by some pixels if the image was not devisable by 8.
4. Some refactoring on the "Adjust mask grow group" order and some notes to clarify it.
5. Fixed "clean_lora_names" group nodes that was not working correctly to clean the lora names. Also removed "logic" multiline to "was".
6. Manual original Seed input "int" was limited changed the node to a Core seed node. Also changed all the "logic" nodes on this part of the workflow to easyuse nodes.
7. Positive and Neg Logic multilne node > Was

---

## \[v7.1] - 30/04/25

### 🐞 Bug Fixes

1. ❗ Fixed: Mask was being resized by "resize image" by KJNodes that was updated and got disconnecter. I changed it to "🔧 Image Crop".
2. ❗ Refactoring AE: New comyUI update broke "Anything Everywhere". When something was connected OF a bypassed group that could not bypass the content, it disconnects the noodle, so in a second run without bypass the workflow would be wrong. Because of that?

   * a. Removed AE from "inpmask" > using Set and Get "inpaint_mask" now
   * b. Removed AE after outpainted group. This means, "main mask" and main "image for inpaint" is now connected directly where they are needed.
   * c. Removed AE from "outpadimage" > using Set and Get "padded_outpainted_image"

---

## \[v7.0]

### ➕ New

* 🔹 Flux Tools LoRA Depth & Canny control-net support (Alimama only; not for Flux Fill).

  * ❗ New group for loading each LoRA model with recommended strength 0.75.
  * ❗ Control-room toggles: Depth vs Canny.
  * ❗ Depth & LoRA groups for InstructPixToPix conditioning.
  * ❗ Preprocessing & reference-image groups for Depth/Canny outpainting.
  * ❗ Localized area option for Canny/Depth outpainting.
* 🔹🔹 Aura Mask added to COMPACT workflow with easy toggles & sliders.
* 🔹🔹🔹 Workflow Cleanup & Improvements:

  1. Improved ✂️ Inpaint Crop (now auto-muted outside COMPACT).
  2. LoRA Stacker via LoraManager node.
  3. Fixed GrowMask 5px for outpainting blend.
  4. Slider for Image Resize Resolution.
  5. Custom node cleanup: replaced crystools switches with easy-use.
  6. Logic operators updated to easy-use.
  7. Inpaint Stitch moved outside localized group (auto-muted).
  8. Metadata improvements: clean LoRA names with 🖌️ and 🎯 emojis.
  9. Enhanced negative prompt saving logic.

10. Improved prompt reader for SAM Detector metadata retrieval.


---

## \[v7.0]

### ➕ New

* 🔹 Flux Tools LoRA Depth & Canny "control-net" support
  (Works with Alimama — ❗❗ it will **NOT** work with Flux Fill ❗❗)

  * ❗ a) Add new group for Loading each LoRA model.

    * They should be loaded all the time and will be dynamically used.
    * You can control each LoRA strength — recommended: **0.75**
  * ❗ b) New toggles on the control-room group:

    * Choose between **Depth OR Canny** (not both)
  * ❗ c) Depth and LoRA groups for LoRA InstructPixToPixConditioning:

    * Allows you to choose when Depth or Canny will start and stop
    * Conditioning falls back to standard **Alimama inpainting**
  * d) New group for preprocessing the image for Depth or Canny
  * e) Canny or Depth reference image for Outpainting:

    * **Depth**: fast-fill in a more complex way
    * **Canny**: pads outpainting with black
  * f) Canny or Depth works with **localized area** option

* 🔹🔹 New: Aura Mask implemented back on the COMPACT workflow with convenient easy-to-use toggle and sliders.

* 🔹🔹🔹 A bunch of smaller Changes:

  1. Fixed old to new “✂️ Inpaint Crop (Improved)”
  2. LoAd Loras: now using "Lora Stacker (LoraManager)" node
     → It’s better and everyone should use LoraManager — fantastic custom node
  3. Fixed: “GrowMask” 5 px not connected on the outpainting fast-fill for better blending
  4. Added a slider for “Image Resize Resolution”
  5. Nodes cleanup:
     → Replaced some “crystools” switch nodes with “easy-use” switches
     → Reducing dependency on custom nodes (some “crystools” may remain)
     → Same gradual cleanup plan for “Logic” nodes
     → Updated control-room logic nodes to “easy-use”
  6. “Logic operator” > “easy-use” for negative thresholding
     → Uses index instead of bool
  7. “Logic operator” > “easy-use” for localized area inpainting
     → Uses index instead of bool
  8. “✂️ Inpaint Stitch (Improved)” moved outside localized area inpainting group (not on COMPACT)
     → It is now auto-muted by a muter
  9. “Get LoRAs names and process it do Get metadata” group updated:
     → Evaluates if LoRAs are in use in a different way
     → Adds clean LoRA names to the positive prompt with 🖌️ and 🎯 for full LoRA syntax
  10. Fixed saving negative prompt on metadata: will first save the retrieved original negative prompt and if that is OFF, it will only save inpainting negative **IF** using thresholding.
  11. It will always evaluate if Positive is empty (if retrieving the original positive prompt fails) and if it is, it uses the inpainting prompt to save in the metadata.
  12. Fixed: for COMPACT: "SD Prompt Reader" won't error when retrieving loaded image original metadata that used **SAM Detector** to make a mask.

---
