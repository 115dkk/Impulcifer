# Impulcifer-py313

이 프로젝트는 [Jaakko Pasanen의 Impulcifer](https://github.com/jaakkopasanen/impulcifer) 프로젝트를 **Python 3.13.2와 호환**되도록 수정한 버전입니다.

## 프로젝트 소개
Impulcifer는 헤드폰에서 스피커 가상화를 위한 바이노럴 룸 임펄스 응답(BRIR)을 생성하는 도구입니다.

일반적으로 헤드폰은 머리 내부에서 소리가 나는데, 이는 게임과 영화뿐만 아니라 음악에도 명백한 단점입니다. 왜냐하면 기본적으로 모든 음향 자료는 스피커를 위해 만들어졌기 때문입니다. 헤드폰용 가상 서라운드 기술은 꽤 오랫동안 존재해 왔지만, 거의 모든 기술이 머리 밖 소리 지역화와 스피커의 자연스러움에 대한 기대를 충족시키지 못합니다. 이는 여러분의 뇌가 다른 사람의 귀나 머리가 아닌 오직 여러분 자신의 귀와 머리로만 소리를 지역화하는 법을 배웠기 때문입니다. 헤드폰에서의 서라운드 사운드는 서라운드 가상화 기술이 여러분의 귀에 맞게 맞춤화되었을 때만 설득력이 있을 수 있습니다. BRIR은 헤드폰에서 최고의 가상 스피커 서라운드를 위한 맞춤형 모델입니다. 올바르게 수행하면 가상화된 스피커는 실제 스피커와 구별할 수 없게 됩니다.

Watch these videos to get an idea what good BRIRs can do. The method used by Smyth Realizer and Creative Super X-Fi
demos is the same what Impulcifer uses.

- [Realiser A16 Smyth Research (Kickstarter project)](https://www.youtube.com/watch?v=3mZhN3OG-tc)
- [a16 realiser](https://www.youtube.com/watch?v=RtY9QIkRJxA)
- [Creative Super X-Fi 3D Immersive Headphone Technology at CES 2018](https://www.youtube.com/watch?v=mAidEm9_JYM)

These demos are trying to make headphones sound as much as possible like the speakers they have in the demo room for a
good [wow](https://www.youtube.com/watch?v=KlLMlJ2tDkg) effect. Impulcifer actually takes this even further because
Impulcifer can do measurements with only one speaker so you don't need access to surround speaker setup and can do room
acoustic corrections which are normally not possible in real rooms with DSP.

## Installing
Impulcifer is used from a command line and requires some prerequisites. These installation instructions will guide you
through installing everything that is needed to run Impulcifer on you own PC.

- Download and install Git: https://git-scm.com/downloads. When installing Git on Windows, use Windows SSL verification
instead of Open SSL or you might run into problems when installing project dependencies.
- Download and install 64-bit [Python 3.8](https://www.python.org/getit/). Make sure to check *Add Python 3.8 to PATH*.
- You may need to install libsndfile if you're having problems with `soundfile` when installing `requirements.txt`.
- On Linux you may need to install Python dev packages  
```bash
sudo apt install python3-dev python3-pip python3-venv
```
- On Linux you may need to install [pip](https://pip.pypa.io/en/stable/installing/)
- On Windows you may need to install
[Microsoft Visual C++ Redistributable for Visual Studio 2015, 2017, or 2019](https://support.microsoft.com/en-us/help/2977003/the-latest-supported-visual-c-downloads)

Rest will be done in terminal / command prompt. On Windows you'll find it by searching `cmd` in Start menu.
You should be able to simply copy and paste in these commands. 

- Git clone Impulcifer. This will create a folder in your home folder called `Impulcifer`. See [Updating](#updating)
for other versions than the latest.  
```bash
git clone https://github.com/jaakkopasanen/Impulcifer
```
- Go to Impulcifer folder.  
```bash
cd Impulcifer
```
- Create virtual environment for the project.  
```bash
python -m venv venv
```
- Activate virtualenv.  
```bash
# On Windows
venv\Scripts\activate
# On Mac and Linux
. venv/bin/activate
```
- Update pip and setuptools
```bash
python -m pip install -U pip
```
- Install required packages.  
```bash
pip install -U -r requirements.txt
```
- Verify installation. You should see help printed if everything went well.  
```bash
python impulcifer.py --help
```

When coming back at a later time you'll only need to activate virtual environment again before using Impulcifer.
```bash
cd Impulcifer
# On Windows
venv\Scripts\activate
# On Mac and Linux
. venv/bin/activate
```

### Updating
Impulcifer is under active development and updates quite frequently. Take a look at the [Changelog](./CHANGELOG.md) to
see what has changed.

Versions in Changelog have Git tags with which you can switch to another version than the latest one:
```bash
# Check available versions
git tag
# Update to a specific version
git checkout 1.0.0
```

You can update your own copy to the latest versions by running:
```bash
git checkout master
git pull
```

required packages change quite rarely but sometimes they do and then it's necessary to upgrade them
```bash
python -m pip install -U -r requirements.txt
```
You can always invoke the update for required packages, it does no harm when nothing has changed.

## Demo
The actual BRIR measurements require a little investment in measurement gear and the chances are that you're here before
you have acquired them. There is a demo available for testing out Impulcifer without having to do the actual
measurements. `data/demo` folder contains five measurement files which are needed for running Impulcifer.
`headphones.wav` has the sine sweep recordings done with headphones and all the rest files are recordings done with
stereo speakers in multiple stages.

You can try out what Impulcifer does by running:
```bash
python impulcifer.py --test_signal=data/sweep-6.15s-48000Hz-32bit-2.93Hz-24000Hz.pkl --dir_path=data/demo 
```
Impulcifer will now process the measurements and produce `hrir.wav` and `hesuvi.wav` which can be used with headphone
speaker virtualization software such as [HeSuVi](https://sourceforge.net/projects/hesuvi/) to make headphones sound like
speakers in a room. When testing with HeSuVi copy `hesuvi.wav` into `C:\Program Files\Equalizer APO\config\Hesuvi\hrir`,
(re)start HeSuVi and select `hesuvi.wav` from the Common HRIRs list on Virtualization tab.

## Measurement
BRIR measurements are done with binaural microphones which are also called ear canal blocking microphones or in-ear
microphones. Exponential sine sweep test signal is played on speakers and the sound is recorded with the microphones at
ear canal openings. This setup ensures that the sound measured by the microphones is affected by the room, your body,
head and ears just like it is when listening to music playing on speakers. Impulcifer will then transform these
recordings into impulse responses, one per each speaker-ear pair.

Guide for doing the measurements yourself and comments about the gear needed to do it can be found in
[measurements](https://github.com/jaakkopasanen/Impulcifer/wiki/Measurements) page of Impulcifer wiki. The whole process
is really quite simple and doesn't take more than couple of minutes. Reading through the measurement guide is most
strongly recommended when doing measurements the first time or using a different speaker configuration the first time.

Following is a quick reference for running the measurements once you're familiar with the process. If you always use
`my_hrir` as the temporary folder and rename it after the processing has been done, you don't have to change the
following commands at all and you can simply copy-paste them for super quick process.

### 7.1 Speaker Setup
Steps and commands for doing measurements with 7.1 surround system:

| Setup | Command |
|-------|---------|
| Put microphones in ears, put headphones on | `python recorder.py --play="data/sweep-seg-FL,FR-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/headphones.wav"` |
| Take heaphones off, look forward | `python recorder.py --play="data/sweep-seg-FL,FC,FR,SR,BR,BL,SL-7.1-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/FL,FC,FR,SR,BR,BL,SL.wav"` |

### Stereo Speaker Setup
Steps and commands for doing measurements with two speakers in four stages:

| Setup | Command |
|-------|---------|
| Put microphones in ears, put on headphones | `python recorder.py --play="data/sweep-seg-FL,FR-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/headphones.wav"` |
| Take heaphones off, look forward | `python recorder.py --play="data/sweep-seg-FL,FR-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/FL,FR.wav"` |
| Look 120 degrees left (left speaker should be on your right) | `python recorder.py --play="data/sweep-seg-FL,FR-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/SR,BR.wav"` |
| Look 120 degrees right (right speaker should be on your left) | `python recorder.py --play="data/sweep-seg-FL,FR-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/BL,SL.wav"` |
| Look directly at the left speaker OR... | `python recorder.py --play="data/sweep-seg-FL-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/FC.wav"` |
| ...Look directly at the right speaker | `python recorder.py --play="data/sweep-seg-FR-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/FC.wav"` |

### Single Speaker
Steps and command for doing measurements with just a single speaker in 7 steps. All speaker measurements use either
`sweep-seg-FL-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav` or
`sweep-seg-FR-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav` depending if the speaker is connected to left or right
cable terminals in the amplifier. These commands assume the speaker is connected to left speaker terminals.

| Setup | Command |
|-------|---------|
| Put microphones in ears, put on headphones | `python recorder.py --play="data/sweep-seg-FL,FR-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/headphones.wav"` |
| Look 30 degrees right of the speaker (speaker on your front left) | `python recorder.py --play="data/sweep-seg-FL-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/FL.wav"` |
| Look directly at the speaker | `python recorder.py --play="data/sweep-seg-FL-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/FC.wav"` |
| Look 30 degrees left of the speaker (speaker on you front right) | `python recorder.py --play="data/sweep-seg-FL-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/FR.wav"` |
| Look 90 degrees left of the speaker (speaker on your right) | `python recorder.py --play="data/sweep-seg-FL-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/SR.wav"` |
| Look 150 degrees left of the speaker (speaker on your back right) | `python recorder.py --play="data/sweep-seg-FL-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/BR.wav"` |
| Look 150 degrees right of the speaker (speaker on you back left) | `python recorder.py --play="data/sweep-seg-FL-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/BL.wav"` |
| Look 90 degrees right of the speaker (speaker on your left) | `python recorder.py --play="data/sweep-seg-FL-stereo-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav" --record="data/my_hrir/SL.wav"` |

## Processing
Once you have obtained the sine sweep recordings, you can turn them into a BRIR file with Impulcifer. All the processing
is done by running a single command on command line. The command below assumes you have made a speaker recordings and
a headphones recording and saved the recording files into `data/my_hrir` folder. Start command prompt, jump to
Impulcifer folder and activate the virtual environment as described in the installation instructions if you don't have
command prompt open yet. Sine sweep recordings are processed by running `impulcifer.py` with Python as shown below.
```bash
python impulcifer.py --test_signal="data/sweep-6.15s-48000Hz-32bit-2.93Hz-24000Hz.pkl" --dir_path="data/my_hrir" --plot
```

You should have several WAV files and graphs in the folder. `hesuvi.wav` can now be used with HeSuVi to make your
headphones sound like speakers.

`--dir_path=data/my_hrir` tells Impulcifer that the recordings can be found in a folder called `my_hrir` under `data`.
Impulcifer will also write all the output files into this folder.

Impulcifer always needs to know which sine sweep signal was used during recording process. Test signal can be either a
WAV (`.wav`) file or a Pickle (`.pkl`) file. Test signal is read from a file called `test.pkl` or `test.wav`. 
`impulse_response_estimator.py` produces both but using a Pickle file is a bit faster. Pickle file however cannot be
used with `recorder.py`. An alternative way of passing the test signal is with a command line argument `--test_signal`
which takes is a path to the file eg. `--test_signal="data/sweep-6.15s-48000Hz-32bit-2.93Hz-24000Hz.pkl"`.

**Python 3.13.2 호환 버전 새로운 기능**: 이제 간단한 이름이나 숫자만으로 테스트 신호를 지정할 수 있습니다:
- `--test_signal="default"` 또는 `--test_signal="1"`: Pickle 형식의 기본 테스트 신호 (sweep-6.15s-48000Hz-32bit-2.93Hz-24000Hz.pkl)
- `--test_signal="sweep"` 또는 `--test_signal="2"`: WAV 형식의 테스트 신호 (sweep-6.15s-48000Hz-32bit-2.93Hz-24000Hz.wav)
- `--test_signal="stereo"` 또는 `--test_signal="3"`: FL,FR 스테레오 테스트 신호
- `--test_signal="mono-left"` 또는 `--test_signal="4"`: FL 모노 테스트 신호
- `--test_signal="left"` 또는 `--test_signal="5"`: FL 스테레오 테스트 신호
- `--test_signal="right"` 또는 `--test_signal="6"`: FR 스테레오 테스트 신호

이 기능을 사용하면 테스트 신호 파일의 정확한 경로를 기억할 필요 없이 간단하게 사용할 수 있습니다:
```bash
# 간단한 이름으로 테스트 신호 지정
impulcifer --test_signal="stereo" --dir_path="data/my_hrir"

# 숫자로 테스트 신호 지정
impulcifer --test_signal="3" --dir_path="data/my_hrir"
```

Sine sweep recordings are read from WAV files which have speaker names separated with commas and `.wav` extension eg.
`FL,FR.wav`. The individual speakers in the given file must be recorded in the order of the speaker names in the file
name. There can be multiple files if the recording was done with multiple steps as is the case when recording 7.1 setup
with two speakers. In that case there should be `FL,FR.wav`, `SR,BR.wav`, `BL,SL.wav` and `FC.wav` files in the folder.

#### Room Correction
Similar pattern is used for room acoustics measurements. The idea is to measure room response with a calibrated
measurement microphone in the exact same spot where the binaural microphones were. Room measurement files have file name
format of `room-<SPEAKERS>-<left|right>.wav`, where `<SPEAKERS>` is the comma separated list of speaker names and
`<left|right>` is either "left" or "right". This tells if the measurement microphone is measuring at the left or right
ear position. File name could be for example `room-FL,FR-left.wav`. Impulcifer does not support stereo measurement
microphones because vast majority of measurement microphones are mono. Room recording files need to be single track
only. Some measurement microphones like MiniDSP UMIK-1 are seen as stereo microphones by Windows and will for that
reason record a stereo file. `recorder.py` can force the capture to be one channel by setting `--channels=1`.

Generic room measurements can be done for speakers with which it's hard to position the measurement microphone
correctly. Impulcifer reads these measurements from `room.wav` file which can contain any number of tracks and any
number of sweeps per track. All the sweeps are being read and their frequency responses are combined. The combined
frequency response is used for room correction with the speakers that don't have specific measurements
(`room-FL,FR-left.wav` etc...).

There are two methods for combining the frequency responses: `"average"` and `"conservative"`. Average method takes the
average frequency response of all the measurements and builds the room correction equalization with that. Conservative
method takes the absolute minimum error for each frequency and only if all the measurements are on the same side of 0
level at the given frequency. This ensures that there will never be room correction adjustments that would make the
frequency response of any of the measurements worse. These methods are available with `--fr_combination_method=average`
and `--fr_combination_method=conservative`.

Upper frequency limit for room measurements can be adjusted with parameters `--specific_limit` and `--generic_limit`.
These will limit the room correction equalization to 0 dB above that frequency. This can be useful for avoiding pitfalls
of room correction in high frequencies. `--specific_limit` applies to room measurements which specify the ear and
`--generic_limit` to room measurements which don't. Typically room dominates the frequency response below 300 or 400 Hz
and speakers dominate above 700 Hz. Speaker's problems cannot be fixed based on in-room measurements and therefore the
limits should usually be placed at 700 Hz. The octave leading up to the limit (eg. 350 to 700 Hz) will be sloped down
from full EQ effect (at 350 Hz) to 0 dB at the limit (700 Hz). Other, higher, values can be tried out and they can
improve the sound but there are not guarantees about that.

Generic room measurements are not expected to be in the same location as the binaural microphones were so limiting the
equalization to less than 1 kHz is probably a good idea. Conservative combination method with several measurements is
safer method and with that it should be safer to try to increase the limit up from 1 kHz. For example
`--specific_limit=5000 --generic_limit=2000` would ensure that room correction won't adjust frequency response of BRIR
above 5 kHz for any speaker-ear pairs and above 2 kHz for speaker-ear pairs that don't have specific room measurements.

Room measurements can be calibrated with the measurement microphone calibration file called `room-mic-calibration.txt`
or `room-mic-calibration.csv`. This must be a CSV file where the first column contains frequencies and the second one
amplitude data. Data is expected to be calibration data and not equalization data. This means that the calibration data
is subtracted from the room frequency responses. An alternative way of passing in the measurement microphone calibration
file is with a command line argument `--room_mic_calibration` and it takes a path to the file eg.
`--room_mic_calibration="data/umik-1_90deg.txt"`

Room frequency response target is read from a file called `room-target.txt` or `room-target.csv`. Head related impulse
responses will be equalized with the difference between room response measurements and room response target. An
alternative way to pass in the target file is with a command line argument `--room_target` eg.
`--room_target="data/harman-in-room-loudspeaker-target.csv"`.

Room correction can be skipped by adding a command line argument `--no_room_correction` without any value.

#### Headphone Compensation
Impulcifer will compensate for the headphone frequency response using headphone sine sweep recording if the folder
contains file called `headphones.wav`. If you have the file but would like not to have headphone compensation, you can
simply rename the file for example as `headphones.wav.bak` and run the command again. 

Headphone equalization can be baked into the produced BRIR file by having a file called `eq.csv` in the folder. The eq 
file must be an AutoEQ produced result CSV file. Separate equalizations for left and right channels are supported with
files `eq-left.csv` and `eq-right.csv`. Headphone equalization is useful for in-ear monitors because it's not possible
to do headphone compensation with IEMs. When using IEMS you still need an around ear headphone for the headphone
compensation. **eq.wav is no longer supported!**

Impulcifer will bake the frequency response transformation from the CSV file into the BRIR and you can enjoy speaker
sound with your IEMs. You can generate this filter with [AutoEQ](https://github.com/jaakkopasanen/AutoEq); see usage
instructions for [using sound signatures](https://github.com/jaakkopasanen/AutoEq#using-sound-signatures) to learn how 
to transfer one headphone into another. In this case the input directory needs to point to the IEM, compensation curve
is the curve of the measurement system used to measure the IEM and the sound signature needs point to the existing
result of the headphone which was used to make the headphone compensation recording.

For example if the headphone compensation recording was made with Sennheiser HD 650 and you want to enjoy Impulcifer
produced BRIR with Campfire Andromeda, you should run:
```bash
python -m autoeq --input-file="measurements/oratory1990/data/in-ear/Campfire Audio Andromeda.csv" --output-dir="my-results/Andromeda (HD 650)" --target="targets/AutoEq in-ear.csv" --sound-signature="results/oratory1990/over-ear/Sennheiser HD 650/Sennheiser HD 650.csv" --equalize --bass-boost=8 --max-gain=12
```
and then copy `AutoEq/my_results/Andromeda (HD 650)/Campfire Audio Andromeda.csv` to `Impulcifer/data/my_hrir/eq.csv`.

See how the Harman over-ear target is used for IEM in this case. This is because the goal is to make Andromeda sound as
similar as possible to HD 650, which is an over-ear headphone. Normally with AutoEQ you would use Harman in-ear target
for IEMs but not in this case.

Headphone compensation can be skipped by adding a command line argument `--no_headphone_compensation` without any value.

#### Sampling Rate
Outputs with different sampling rates than the recording sampling rate can be produced with `--fs` parameter. This
parameter takes a sampling rate in Hertz as value and will then resample the output BRIR to the desired sampling rate if
the recording and output sampling rates differ. For example `--fs=44100`.

#### Plotting Graphs
Various graphs can be produced by providing `--plot` parameter to Impulcifer. These can be helpful in figuring out what
went wrong if the produced BRIR doesn't sound right. Producing the plots will take some time.

- **pre** plots are the unprocessed BRIR measurement
- **room** plots are room measurements done with measurement microphone
- **post** plots are the final results after all processing

#### Channel Balance Correction
Channel balance can be corrected with `--channel_balance` parameter. In ideal case this would not be needed and the
natural channel balance after headphone equalization and room correction would be perfect but this is not always the
case since there are multiple factors which affect that like placement of the binaural microphones. There are six
different strategies available for channel balance correction.

Setting `--channel_balance=trend` will equalize right side by the difference trend of left and right sides. This is a
very smooth difference curve over the entire spectrum. Trend will not affect small deviations and therefore doesn't
warp the frequency response which could lead to uncanny sensations. Bass, mids and treble are all centered when using
trend. Trend is probably the best choice in most situations.

Setting `--channel_balance=mids` will find a gain level for right side which makes the mid frequencies (100, 3000)
average level match that of the left side. This is essentially an automatic guess for the numeric strategy value.

Setting `--channel_balance=1.4` or any numerical value will amplify right side IRs by that number of decibels.
Positive values will boost right side and negative values will attenuate right side. You can find the correct value by
trial and error either with Impulcifer or your virtualization software and running Impulcifer again with the best value.
Typically vocals or speech is good reference for finding the right side gain value because these are most often mixed
in the center and therefore are the most important aspect of channel balance.

Setting `--channel_balance=avg` will equalize both left and right sides to the their average frequency response and
`--channel_balance=min` will equalize them to the minimum of the left and right side frequency response curves. Using
minimum instead of average will be better for avoiding narrow spikes in the equalization curve but which is better in
the end varies case by case. These strategies might cause uncanny sensation because of frequency response warping.

`--channel_balance=left` will equalize right side IRs to have the same frequency response as left side IRs and
`--channel_balance=right` will do the same in reverse. These strategies might cause uncanny sensation because of
frequency response warping.

#### Level Adjustment
Output BRIR level can be adjusted with `--target_level` parameter which will normalize the BRIR gain to the given
numeric value. The level is calculated from all frequencies excluding lowest bass frequencies and highest treble
frequencies and then the level is adjusted to the target level. Setting `--target_level=0` will ensure that BRIR
average gain is about 0 dB. Keep in mind that there often is large variance in the gain of different frequencies so
target level of 0 dB will not mean that the BRIR would not produce clipping. Typically the desired level is several dB
negative such as `--target_level=-12.5`. Target level is a tool for having same level for different BRIRs for easier
comparison.

#### Decay Time Management
The room decay time (reverb time) captured in the binaural room impulse responses can be shortened with `--decay`
parameter. The value is a time it should take for the sound to decay by 60 dB in milliseconds. When the natural decay
time is longer than the given target, the impulse response tails will be shortened with a slope to achieve the desired
decay velocity. Decay times are not increased if the target is longer than the natural one. The decay time management
can be a powerful tool for controlling ringing in the room without having to do any physical room treatments.

## Contact
[Issues](https://github.com/jaakkopasanen/AutoEq/issues) are the way to go if you are experiencing problems, have
ideas or if there is something unclear about how things are done or documented.

You can find me in [Reddit](https://www.reddit.com/user/jaakkopasanen) and
[Head-fi](https://www.head-fi.org/members/jaakkopasanen.491235/) if you just want to say hello.

There is also a [Head-fi thread about Impulcifer](https://www.head-fi.org/threads/recording-impulse-responses-for-speaker-virtualization.890719/).

## Python 3.13.2 호환성 개선 사항
이 프로젝트는 원본 Impulcifer의 코드를 Python 3.13.2에서 작동하도록 다음과 같은 부분을 수정했습니다:

1. **autoeq-py313 의존성 추가**: Python 3.13.2와 호환되는 AutoEq 버전을 사용하도록 수정
2. **설치 방법 간소화**: PyPI를 통한 설치 방법 추가

## 라이센스
이 프로젝트는 원본 Impulcifer와 동일하게 MIT 라이센스를 따릅니다. 원본 저작권은 Jaakko Pasanen에게 있습니다.

```
MIT License

Copyright (c) 2018-2022 Jaakko Pasanen
Copyright (c) 2023 Python 3.13.2 호환 버전 제작자

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.