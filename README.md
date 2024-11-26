TypeError: (0 , _ffmpeg_ffmpeg__WEBPACK_IMPORTED_MODULE_11__.createFFmpeg) is not a function

This error happened while generating the page. Any console logs will be displayed in the terminal window.
Source
components\demoCreationComponents\audio2json\audiotoJson.jsx (18:28) @ eval

  16 | import { createFFmpeg, fetchFile } from '@ffmpeg/ffmpeg';
  17 | 
> 18 | const ffmpeg = createFFmpeg({ log: true });
     |                          ^
  19 | 
  20 | const AudiotoJson = () => {
  21 | const [notification, setNotification] = useState('');
