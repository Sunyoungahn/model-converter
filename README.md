# Model converter

PyTorch (.pth) → ONNX → TensorFlow Lite (.tflite) Conversion Pipeline

**PyTorch로 학습된 모델(.pth)**을 ONNX → TensorFlow(.pb/SavedModel) → TensorFlow Lite(.tflite) 순서로 변환



🛠 Requirements
	•	Python 3.11
	•	PyTorch 2.x
	•	ONNX & ONNX Runtime
	•	TensorFlow 2.x
	•	onnx2tf


먼저 학습한 모델을 .pth , checkpoint만 저장한다. 
유연성을 위해 모델 전체를 저장하지는 않기로 한다. 


# Usage and Issues

1. pth → onnx 

대상 모델이 Multi Input Model이면 변환시 주의할것


2. onnx → tensorflow (.pb) 

 ONNX → TensorFlow 변환 시 NCHW → NHWC


3. tensorflow → tflite 변환


