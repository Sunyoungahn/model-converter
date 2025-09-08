# Model 변환 Project

pytorch 학습 모델을 디바이스 로컬에 설치하여 추론할수 있도록.


# Trial and errors

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

1. pth → onnx 

대상 모델이 Multi Input Model이면 변환시 주의할것


2. onnx → tensorflow (.pb) 

 ONNX → TensorFlow 변환 시 NCHW → NHWC


3. tensorflow → tflite 변환


# update
onnx 로변경하려니 값이 안맞음.
bert onnx input 값에 전처리가 필요하고
그 전처리는 jamo, g2pkk라는 파이선 라이브러리를 사용한다. -> 모바일로 포팅이 가능한가? -> 해당 언어로 포팅 할것


# TTS 추론 파이프라인 설명
 
 🔄 전체 과정:
 텍스트 입력 → G2P 변환 → BERT 처리 → TTS 모델 추론 → 오디오 출력
 
 📝 1. 텍스트 처리 (Text Processing):
 - 입력된 한국어 텍스트를 정규화
 - G2pKK를 사용해 한글을 음소(phoneme)로 변환
 - 예: "안녕하세요" → "ㅇㅏㄴㄴㅕㅇㅎㅏㅅㅔㅇㅛ"
 - 음소를 ID로 매핑하여 모델이 이해할 수 있는 숫자 배열로 변환
 
 🧠 2. BERT 처리 (BERT Processing):
 - 한국어 BERT 모델을 사용해 텍스트의 의미적 특징 추출
 - 각 음소에 대응하는 768차원 벡터 생성
 - 자연스러운 억양과 감정 표현을 위한 컨텍스트 정보 제공
 
 🎵 3. TTS 추론 (TTS Inference):
 - MeloTTS 모델이 음소 ID + BERT 특징을 입력으로 받음
 - Transformer 기반 아키텍처로 음향 특징(mel-spectrogram) 생성
 - Vocoder를 통해 음향 특징을 실제 오디오 파형으로 변환
 - 출력: 44100Hz 샘플레이트의 Float 배열 (PCM 오디오)
 
 ⚡ 성능 최적화:
 - ONNX Runtime 사용으로 추론 속도 향상
 - INT8 양자화 모델로 메모리 사용량 감소
 - CPU 최적화로 모바일 환경에서도 빠른 처리
 
 🔊 오디오 재생:
 - Float 배열을 16-bit PCM WAV 형식으로 변환
 - AVAudioPlayer를 통해 iOS에서 재생
 - 실시간 오디오 길이 및 품질 측정
 
 📊 품질 지표:
 - 생성 시간: 일반적으로 1초 미만
 - 오디오 품질: 22050Hz → 44100Hz 업샘플링
 - 자연스러운 한국어 발음과 억양 구현
 
 💡 주요 특징:
 - 한국어 연음 규칙 지원 (예: "음성이" → "음성이" 아닌 "음서기")
 - 다양한 문장 유형 지원 (평서문, 의문문, 감탄문)
 - 실시간 처리 가능한 경량화된 모델
 - 오프라인 완전 동작 (인터넷 연결 불필요)

