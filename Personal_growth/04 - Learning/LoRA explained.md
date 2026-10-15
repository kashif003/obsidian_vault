

---



LoRa: low rank adaptation.


Precison and Quantization:

+ usually the weights in a model are stored in float32 datatype which has 32 values but we can switch to float16 which is half of precision of float32.

+  rounding in termns of model size, from float32 to float16 will result in loss of the model accuracy. 


+ Quantiztion  means converting one type of dtype to another. i.e less memory.


Note: smaller precision inceases the flops.


Fine tuning techinques:

1. adaptive layers: inseart new layers inside the model and fintune those.
2. LoRa: discussed below.


---
LoRa:

Rank -> how many independent rows or columns exist in the matrix.

rank is smaller than dimentions.

adaptation -> finetuning process


+ In theory the larger models needs less params to fine tune using LoRa technique.

+ lora is typicall added to attention mechanism.


+ lora is uses scalling factor apha/rank where alpha.


--- 
 - [ ] Need to learn more and learn by practicing.

