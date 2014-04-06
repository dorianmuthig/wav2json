wav2json for Windows
====================

This is a fork of wav2json which includes Visual Studio 2013 project files and is ready to compile on Windows.

### Dependencies

Boost Library: http://sourceforge.net/projects/boost/files/boost-binaries/1.55.0/

Mega-Nerd libsndfile: http://www.mega-nerd.com/libsndfile/files/

### About

Generate waveformjs.org compatible json data out of audio files.



Original author: Benjamin Schulz

email: beschulz@betabugs.de

License: GPL



~~Note: the file examples/waveform.js is from http://waveformjs.org/ ~~

### Examples

    wav2json song.wav --channels left right -o song.json

might produce output like

    {
        "left":[0,0.499969,0.865997,0.999969,0.865997,0.499969,0,0.5,0.866028,0,0.866028,0.5],
        "right":[0,0.499969,0.865997,0.999969,0.865997,0.499969,0,0.5,0.866028,0,0.866028,0.5],
    }

Also take a look at some [example renderings](http://beschulz.github.com/wav2json/).

### Performance
Similar to wav2png (e.g. very good). But it might be a little slower, when generating output for multiple channels.
Since the input file is opened multiple times, I'd recommend to write it to a temporary file first, so you can take advantage of the operating systems file cache:

    lame --decode ../song.mp3 tmp.wav
    wav2json tmp.wav --channels left right mid side min max -o song.json
    del tmp.wav

Also note, that you can dramatically reduce the file size, by reducing the precision of the output. In my tests, even a precision of 1 looked good.

### Installation

#### Use the prebuilt binaries

1. Download the appropriate version from the bin\Release directory
2. Copy wav2json.exe and libsndfile-1.dll to any directory of yor liking and add that directpry to the PATH system variable


#### Build it yourself:

##### Install dependencies
- Download the Boost C++ Libraries 1.55.0 binaries from http://sourceforge.net/projects/boost/files/boost-binaries/1.55.0/boost_1_55_0-bin-msvc-all-32-64.7z/download and unpack the file to C:\Program Files\boost\
- Download and install the libsndfile Windows binaries from http://www.mega-nerd.com/libsndfile/#Download

##### Build
Open the Wav2Json solution file in Visual Studio and select Build -> Build Solution or simply press Ctrl+Shift+B

### Usage

    wav2json version 0.7
    written by Benjamin Schulz (beschulz@betabugs.de)

    usage: wav2json [options] input_file_name
    example: wav2json my_file.wav

    Allowed options:

    Generic options:
      -v [ --version ]      print version string
      --help                produce help message

    Configuration:
      -s [ --samples ] arg (=800)           number of samples to generate
      --channels arg (=left right )         channels to compute: left, right, mid, 
                                            side, min, max
      -p [ --precision ] arg (=6)           precision of the floats, that are 
                                            generated. [1..6], reduce for smaller 
                                            sized files. Usually 2 should be 
                                            sufficient!
      -o [ --output ] arg                   name of output file, defaults to <name 
                                            of inputfile>.json
      -c [ --config-file ] arg (=wav2json.cfg)
                                            config file to use
      -d [ --db-scale ]                     use logarithmic (e.g. decibel) scale 
                                            instead of linear scale
      --db-min arg (=-48)                   minimum value of the signal in dB, that
                                            will be visible in the waveform
      --db-max arg (=0)                     maximum value of the signal in dB, that
                                            will be visible in the waveform. 
                                            Useful if you know that your signal 
                                            peaks at a certain level.
      -n [ --no-header ]                    Do not include the version info banner 
                                            in the output


One thing, thats noteworthy is, that you can generate output for multiple channels:

* left is the left channel of the audio file.
* right is the right channel of the audio file
* mid is the mid channel of the audio file computed per sample as: mid = (left + right) / 2
* side is the side channel of the audio file computed per sample as: side = (left + right) / 2
* max is the maximum of the channels of the audio file computed per sample as: max = max(left, right)
* min is the minimum of the channels of the audio file computed per sample as: min = min(left, right)

Obviously outputting the right, mid, side, min, max channels does only make sense on stereo input. When the input is mono,
the applications falls back to outputting the left channel and prints a warning to stderr.

When there are more than two channels present, the output might be a little non-sensical, depending on what information is stored in the first and second channel.

Also note, that if there are less samples in the audio file, than defined by --samples, only as many samples as there are in the audio file will be written to the json file. The interpolation should be done by waveform.js

### Donations
Please don't try to send me money. Money is the root of all evil. It would only get me in trouble with my bank or tax regulation authorities. Also, I didn't do most of the work on the project, so that hardly seems fair.