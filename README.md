# mfcc-python
Mel Frequency Cepstral Coefficients - build MFCC API on python

# Description
Create REST API to extract Mel Frequency Cepstral Coefficients (MFCC) from audio file. API implemented on python and build using Flask on local server. Clients are able to get MFCC output values from audio files and parameters (frame length, frame shift and number of mel banks) uploaded using POST request to server.  

# How to Run
This project has been tested to run using Python v3.8.10. Please install compatible version of python on your machine before moving forward.

This projects are built on top of python virtual environtment (venv) to isolate package/libraries installation, there will be slight changes to project folder on Windows and Linux/Mac OS

## If you are using Windows
Please use mfcc-windows folder. Before running the server, change the current working directory to mfcc-windows folder and run ```activate.bat``` to activate virtual environment, initialize dependencies installation using pip and run server.py (if you skip this, there will be an 'module not found error' message). After several seconds, the server will run on http://127.0.0.1:5000
```
\mfcc_windows>activate

 * Serving Flask app 'server' (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000 (Press CTRL+C to quit)
127.0.0.1 - - [21/Apr/2022 19:57:00] "POST / HTTP/1.1" 200 -
```

## If you are using Linux/MacOS
Please use mfcc-windows folder. Before running the server, change the current working directory to mfcc_linux folder and run activate.sh to activate virtual environment, initialize dependencies installation using pip and run ```server.py``` (if you skip this, there will be an 'module not found error' message). After several seconds, the server will run on http://127.0.0.1:5000

```
~/mfcc_linux$ source activate

 * Serving Flask app 'server' (lazy loading)
 * Environment: production
   WARNING: This is a development server. Do not use it in a production deployment.
   Use a production WSGI server instead.
 * Debug mode: off
 * Running on http://127.0.0.1:5000 (Press CTRL+C to quit)
127.0.0.1 - - [21/Apr/2022 19:57:00] "POST / HTTP/1.1" 200 -
```
## Dependencies
Required libraries included in requirements.txt, automatically installed when running ```activate.bat``` / ```activate.sh script```
- scipy == 1.8.0
- flask == 2.1.1
- librosa == 0.9.1
- numpy == 1.21
- requests == 2.26.0

# How to Request into API
Endpoint on ('/') will receive POST request. 

To get MFCC value response from API, users need to include files on body as part of POST request

## Audio file
Users upload audio file (.wav) into body request as parts of the multipart/form-data. Please use 'file' as key and audio file (.wav) as value.

## JSON file
Users may upload json file (.json) into body request as parts of the multipart/form-data. Pleae use 'parameters' as key and JSON file as value.
Users are able to post parameters value (frame length, frame shift and number of mel banks) as json file. For example :
```
{
  "frame_length":25,
  "frame_shift":10,
  "num_mel_bins":26
}
```
## Example : Request using python script
This script was used to test API request using another script on client. 

To process this : Open another terminal, change directory into the project folder and you can find ```client.py```. Before running the script, activate virtual environment (if have not done) to make sure libraries can be used.
```
\mfcc_windows>Scripts/activate

```

```
~/mfcc_linux$ source bin/activate
```


And then run the script
```
\mfcc_windows>python client.py
```
```
~/mfcc_linux$ python client.py
```
You will get JSON formatted respond 
```
{"audio_sample.wav":"[[-1154.813232421875, 1098.047119140625, -409.96728515625, 64.57443237304688, -378.0357971191406, 180.73233032226562, -116.8297348022461, 126.26396179199219, -161.9350128173828, 114.66415405273438, -29.271366119384766, 132.44741821289062, -112.46929168701172, 9.693145751953125,....
```

You can upload multiple audio files per request, to do that, open ```cliet.py``` -> add another file from local directory to this list
```
#Audio file to be sent
audio_file = ['sources/audio_sample.wav']
```

For parameters (frame length, frame shift and number of mel banks) inside json file, you only allowed to upload single json file, to change parameter value open ```cliet.py``` -> add another file from local directory to this list
```
local_json = 'sources/parameters.json'
json_file = json.load(open(local_json))
```

All local files saved on ```sources``` folder inside project directory

## Example : Request using Postman Application
If you have Postman installed on your machine, you can initiate POST request into http://127.0.0.1:5000. Choose ```Body->form-data```. Put audio files as file format with key 'file' (you can upload multiple audio files as long as the key is the same) and put parameters inside json files using file format with key 'parameters' (you only allowed to upload one json file per request). 

![Postman](https://github.com/maulanaisa/mfcc-python/blob/main/images/postman.png)

## Respond Output
You can expect json format (content-type header 'application/json') after successful request to API server with format below :

```
{
   "fileName1" : "13xN1 matrix(string)",
   "fileName2" : "13xN2 matrix(string)",
   "fileName3" : "13xN3 matrix(string)",
   ...
 }
```

