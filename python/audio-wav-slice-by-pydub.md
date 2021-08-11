# wav 音频分割

## 环境

* ffmpeg 环境
* pydub 包  `pip install pydub`

```python
from pydub import AudioSegment


def get_second_part_wav(main_wav_path, start_time, end_time, part_wav_path):
    """
    音频切片，获取部分音频，单位秒
    :param main_wav_path: 原音频文件路径
    :param start_time: 截取的开始时间
    :param end_time: 截取的结束时间
    :param part_wav_path: 截取后的音频路径
    :return:
    """
    start_time = start_time * 1000
    end_time = end_time * 1000

    sound = AudioSegment.from_mp3(main_wav_path)
    word = sound[start_time:end_time]

    word.export(part_wav_path, format="wav")


if __name__ == '__main__':
    wav_path = "t.wav"
    part_path = "t1.wav"
    s = 0
    e = 5.33333
    get_second_part_wav(wav_path, s, e, part_path)
```

