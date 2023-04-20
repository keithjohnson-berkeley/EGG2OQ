# EGG2OQ

Measure glottal Open Quotient (OQ) from ElectroGlottography (EGG)

Assume we have a stereo audio file, where the acoustic waveform is in the left channel, and an electroglottograph (EGG) signal is in the right channel (Herbst, 2020).

This notebook illustrates some processing steps to calculate the vocal fold vibration open quotient (OQ) from the EGG signal using the 'hybrid' method.

Christian T. Herbst (2020) "Electroglottography − An Update." *Journal of Voice*, **34**(4), pp. 503-526.


Two functions are declared in this notebook.  process_EGG takes a filename of an audio/EGG recording and returns three numpy arrays - times of the analysis frames in seconds, f0 of glottal vibration in each frame (the first measurable pitch pulse in the frame), and the OQ in each frame (again first pulse).

    process_EGG(filename,egg_channel=1,window_length = 0.02, hop_dur = 0.005,
                hp_cut = 70, hp_order = 8, peak_sep = 0.0055,threshold=0.43):
      '''
      Inputs:
          filename - name of a two channel audio file with EGG in one of the channels
          egg_channel - audio channel (0 or 1) where EGG signal will be found
          window_length - duration of analysis frames (default=0.02 seconds)  
          hop_dur - interval between frames (default=0.005 seconds)
          hp_cut - highpass filter cut frequency (default=70)
          hp_order - highpass filter order (default=8)
          peak_sep - required separation between glottal closing instants (default = 0.0055 seconds)
          threshold - on the normalized EGG signal used to find the glottal opening instant (default = 0.43)
        
     Outputs:
          time - midpoint times (seconds) of the analysis frames in f0 and OQ
          f0 - estimates of the fundamental frequency of voicing
          OQ - the open quotient: proportion of the glottal cycle in which the glottis is open
        
     '''
 
 The second function is a small helper function to create analysis frames from a 1D array of samples.
 
    
    make_frames(array, winsize, stride)
      '''
      takes a 1D numpy array (one channel of audio data for example), and returns a 2D
      array of frames. each frame is 'winsize' long, and the starting points of
      the frames are at intervals of 'stride'.  'winsize' and 'stride' must be given 
      in number of samples.  Here is an example call (where fs is the sampling rate):

      frames = make_frames(signal,int(0.015*fs),int(0.005*fs))  # 15ms windows, 5ms stride
      '''
    
 For example, a six sample waveform split into frames of three samples each, with a stride of one sample between each frame gives this:

    [1,2,3,4,5,6] --> [[1,2,3],
                       [2,3,4],
                       [3,4,5],
                       [4,5,6]]
                     

In this example the first frame is [1,2,3].


 
    
