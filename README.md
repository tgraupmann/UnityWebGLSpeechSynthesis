# UnityWebGLSpeechSynthesis
Unity WebGL Package For Speech Synthesis

# Target

The `Unity WebGL Speech Synthesis Package` is created for Unity version `5.5` or better.
This package is **only** intended for the `WebGL` platform and requires a browser with the built-in [Web Speech API](https://dvcs.w3.org/hg/speech-api/raw-file/tip/speechapi.html), like Chrome.
Speech Synthesis requires an Internet connection.
Check the [browser compatibility](https://developer.mozilla.org/en-US/docs/Web/API/Web_Speech_API#Browser_compatibility) to see which browsers implemented the `Speech API`.

## Supported Browsers

* Chrome
* Edge
* Firefox
* Safari

# Changelog

1.0 - Initial creation of the project

# Demos

[Demo 01 Unity Speech Synthesis](https://theylovegames.com/UnityWebGLSpeechSynthesis_01Synthesis/)

# Documentation

This document can be accessed in `Assets/WebGLSpeechSynthesis/Readme.pdf` or use the menuitem `GameObject->WebGLSpeechSynthesis->Online Documentation`

# Quick Start

1 Switch to the `WebGL` platform in `Build Settings [image_2](images/image_2.png)

2 Create one `WebGLSpeechSynthesisPlugin` GameObject in the scene with the menu `GameObject->WebGLSpeechSynthesis->Create WebGLSpeechSynthesisPlugion` [image_3](images/image_3.png)

3 (Optional) You may need a voices dropdown in your UI, use the menuitem `GameObject->WebGLSpeechSynthesis->Create Voices Dropdown` [image_4](images/image_4.png)

4 At this point you should have a scene with the `WebGLSpeechSynthesisPlugin`, and (optionally) a voices dropdown added to the canvas.

![image_5](images/image_5.png)

5 Create a custom MonoBehaviour script to use the `WebGLSpeechSynthesis` API

6 Add a using statement to get access to the `WebGLSpeechSynthesis` namespace

```
using UnityWebGLSpeechSynthesis;
```

## Speech Synthesis Plugin Quick Setup

7 Add a meta reference for `WebGLSpeechSynthesisPlugin` to the script

```
        /// <summary>
        /// Reference to the plugin
        /// </summary>
        public WebGLSpeechSynthesisPlugin _mWebGLSpeechSynthesisPlugin = null;
```

8 In the `start event` check if the plugin is available.

```
        // Use this for initialization
        void Start()
        {
            // will indicate if the plugin is available
            _mIsAvailable = false;

            // check the meta reference to the plugin
            if (_mWebGLSpeechSynthesisPlugin)
            {
                // check if the plugin is available
                _mIsAvailable = _mWebGLSpeechSynthesisPlugin.IsAvailable();
            }

            // if not available, return
            if (!_mIsAvailable)
            {
                // show a warning
                return;
            }
        }
```

## Speak Quick Setup

9 Add a field to hold the utterance that will be spoken

```
        /// <summary>
        /// Reference to the utterance which holds the voice and text to speak
        /// </summary>
        private WebGLSpeechSynthesisPlugin.SpeechSynthesisUtterance _mSpeechSynthesisUtterance = null;
```

10 Create an instance of `SpeechSynthesisUtterance`

```
        void Start()
        {
            ...

            if (!_mIsAvailable)
            {
                return;
            }

            ...

            // Create an instance of SpeechSynthesisUtterance
            _mSpeechSynthesisUtterance = _mWebGLSpeechSynthesisPlugin.CreateSpeechSynthesisUtterance();
        }
```

11 Speak the utterance

```
        /// <summary>
        /// Speak the utterance
        /// </summary>
        private void Speak()
        {
            if (null == _mWebGLSpeechSynthesisPlugin)
            {
                Debug.LogError("Plugin is not set!");
                return;
            }
            if (null == _mInputField)
            {
                Debug.LogError("InputField is not set!");
                return;
            }
            if (null == _mSpeechSynthesisUtterance)
            {
                Debug.LogError("Utterance is not set!");
                return;
            }

            // Cancel if already speaking
            _mWebGLSpeechSynthesisPlugin.Cancel();

            // Set the text that will be spoken
            _mSpeechSynthesisUtterance.SetText(_mInputField.text);

            // Use the plugin to speak the utterance
            _mWebGLSpeechSynthesisPlugin.Speak(_mSpeechSynthesisUtterance);
        }
```

## Voice Selection Quick Setup

12 Add a field to hold the available voices

```
        /// <summary>
        /// Reference to the supported voices
        /// </summary>
        private WebGLSpeechSynthesisPlugin.VoiceResult _mVoiceResult = null;
```

13 Start a coroutine to get the available voices

```
        void Start()
        {
            ...

            if (!_mIsAvailable)
            {
                return;
            }

            // Get voices from plugin
            StartCoroutine(GetVoices());
        }
```

14 In the coroutine, use the plugin to get the voices

```
        private IEnumerator GetVoices()
        {
            if (!_mIsAvailable)
            {
                yield break;
            }
            _mVoiceResult = null;
            while (null == _mVoiceResult)
            {
                yield return new WaitForSeconds(0.25f);
                _mVoiceResult = _mWebGLSpeechSynthesisPlugin.GetVoices();
            }
        }
```

15 Populate the voices dropdown using the voice result

```
            // prepare the voices drop down items
            Utils.PopulateVoicesDropdown(_mDropDownVoices, _mVoiceResult);
```

16 Handle voice change events from the dropdown

```
            // drop down reference must be set
            if (_mDropdownVoices)
            {
                // set up the drop down change listener
                _mDropdownVoices.onValueChanged.AddListener(delegate {
                    // handle the voice change event, and set the voice on the utterance
                    Utils.HandleVoiceChanged(_mDropdownVoices,
                        _mVoiceResult,
                        _mSpeechSynthesisUtterance);
                    // Speak in the new voice
                    Speak();
                });
            }
```

# Scenes

## Example01 - Speech Synthesis

The scene is located at `Assets/WebGLSpeechSynthesis/Scenes/Example01_Synthesis.unity`

![image_1](images/image_1.png)

# Support

Send questions and/or feedback to the support@theylovegames.com email.
