# Installation-of-Dependencies on Ubuntu 
Here, installing Opencv 3.3 and Python, Keras, Caffe.
================================================================================================


STEP 1:   UPDATE THE SYSTEM PACKAGES
-------------------------------------
        sudo apt-get update
        sudo apt-get upgrade

STEP 2:   INSTALL UBUNTU DEPENDENCY LIBRARIES FOR OPENCV AND PYTHON
------------------------------------------------------------------
        sudo apt-get install build-essential checkinstall cmake pkg-config yasm
        sudo apt-get install git gfortran
        sudo apt-get install libjpeg8-dev libjasper-dev libpng12-dev
 
     # If you are using Ubuntu 14.04
     ................................
        sudo apt-get install libtiff4-dev

     # If you are using Ubuntu 16.04
     ...............................
        sudo apt-get install libtiff5-dev
 
        sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libdc1394-22-dev
        sudo apt-get install libxine2-dev libv4l-dev
        sudo apt-get install libgstreamer0.10-dev libgstreamer-plugins-base0.10-dev
        sudo apt-get install qt5-default libgtk2.0-dev libtbb-dev
        sudo apt-get install libatlas-base-dev
        sudo apt-get install libfaac-dev libmp3lame-dev libtheora-dev
        sudo apt-get install libvorbis-dev libxvidcore-dev
        sudo apt-get install libopencore-amrnb-dev libopencore-amrwb-dev
        sudo apt-get install x264 v4l-utils
 
     # Other Optional dependencies
     .................................      
        sudo apt-get install libprotobuf-dev protobuf-compiler
        sudo apt-get install libgoogle-glog-dev libgflags-dev
        sudo apt-get install libgphoto2-dev libeigen3-dev libhdf5-dev doxygen


Step 3 : Install Python libraries
-------------------------------------------
        sudo apt-get install python-dev python-pip python3-dev python3-pip
        sudo -H pip2 install -U pip numpy
        sudo -H pip3 install -U pip numpy
      
      # Here we use seperate virtual environment for python2 and python3
      # Note: ubuntu 14 and above version comes with these two variant of python by default  
      -------------------------------------------------------------------------------------
          # Install virtual environment
          ..............................
          sudo -H pip2 install virtualenv virtualenvwrapper
          sudo -H pip3 install virtualenv virtualenvwrapper
          echo "# Virtual Environment Wrapper"  >> ~/.bashrc
          echo "source /usr/local/bin/virtualenvwrapper.sh" >> ~/.bashrc
          source ~/.bashrc
  
          ############ For Python 2 ############
     |     
     |     # create virtual environment
     |     mkvirtualenv cv2 -p python2  #here cv2 is user wish, u can give any name for virtual environment.be 
     smart to remember it.
     |     workon cv2  
     |     
     |     # now install python libraries within this virtual environment
     |     pip install numpy scipy matplotlib scikit-image scikit-learn ipython
     |
     |     # quit virtual environment
     |     deactivate
          ######################################
  
          ############ For Python 3 ############
     |     # create virtual environment
     |     mkvirtualenv cv3 -p python3   #here cv3 is user wish, u can give any name for virtual environment.be
     smart to remember it.
     |     workon cv3
     |  
     |     # now install python libraries within this virtual environment
     |     pip install numpy scipy matplotlib scikit-image scikit-learn ipython
     |
     |      # quit virtual environment
     |      deactivate
           ######################################
           
Step 4: DOWNLOAD OPENCV 3.3  WHICH COMES WITH OpenCV_contrib         
-------------------------------------------------------------
      #TO DOWNLOAD OPENCV
      ...................
      git clone https://github.com/opencv/opencv.git
      cd opencv 
      git checkout 3.3.1 
      cd ..
      #TO DOWNLOAD OPENCV_contrib
      ...........................
      git clone https://github.com/opencv/opencv_contrib.git
      cd opencv_contrib
      git checkout 3.3.1
      cd ..
      
Step 5: COMPILE OPENCV          
-----------------------
      cd opencv
      mkdir build
      cd build
      
      #Here compilation done using CMAKE
      ..................................
      cmake -D CMAKE_BUILD_TYPE=RELEASE \
            -D CMAKE_INSTALL_PREFIX=/usr/local \
            -D INSTALL_C_EXAMPLES=ON \
            -D INSTALL_PYTHON_EXAMPLES=ON \
            -D WITH_TBB=ON \
            -D WITH_V4L=ON \
            -D WITH_QT=ON \
            -D WITH_OPENGL=ON \
            -D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib/modules \
            -D BUILD_EXAMPLES=ON ..
            
      # find out number of CPU cores in your machine
      nproc
      
      # substitute 4 by output of nproc
      make -j4
      sudo make install
      sudo sh -c 'echo "/usr/local/lib" >> /etc/ld.so.conf.d/opencv.conf'
      sudo ldconfig
      
Step 6: LINK OPENCV WITH PYTHON (VERY IMPORTANT)
-------------------------------------------------
      #Depending upon Python version you have, paths would be different.
      #OpenCV’s Python binary (cv2.so) can be installed either in directory site-packages or dist-packages. 
      #Use the following command to find out the correct location on your machine.
      ............................................................................
      
      # Inorder to find where our cv2.so file located
      # Below command should output paths similar to one of these (or two in case OpenCV was compiled for
      both Python2 and  Python3):
      ................................................................................................................
      find /usr/local/lib/ -type f -name "cv2*.so"
      
      #Double check the exact path on your machine before running the following commands
      .....................................................................................
      ############ For Python 2 ############
      ......................................................................
      cd ~/.virtualenvs/cv2/lib/python2.7/site-packages
      ln -s /usr/local/lib/python2.7/dist-packages/cv2.so cv2.so
  
      ############ For Python 3 ############
      ......................................................................
      cd ~/.virtualenvs/cv3/lib/python3.6/site-packages
      ln -s /usr/local/lib/python3.6/dist-packages/cv2.cpython-36m-x86_64-linux-gnu.so cv2.so
      ...........
      Note: If error occurs while linking the opencv with python3, kindly check the location of cv2.so and 
      cv2.cpython-36m-x86_64-linux-gnu.so (in certain case the file name will be cv2.cpython-35m-x86_64-linux-gnu.so,
      which is based on python version) if ur system has python 3.5 as defaults, use the below command :
      ...................................
      cd ~/.virtualenvs/cv3/lib/python3.5/site-packages
      ln -s /usr/local/lib/python3.5/dist-packages/cv2.cpython-35m-x86_64-linux-gnu.so cv2.so     
===========================================================================================================================     

NOW WE HAVE INSTALLED OPENCV AND LINKED IT WITH PYTHON
===========================================================================================================================     
 STEP 7 : TESTING
 ------------------------
      workon cv2                     # or else use "workon cv3" to check python3    
      python
      import cv2                     #if cv2 imported successfully,then you may installed everything cleanly
      print cv2.__version__          #for some python version use "print(cv2.__version__)"
      
      
=============================================================================================================================
\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\\
=============================================================================================================================

INSTALLATION OF KERAS (on the day keras 2.0 was availble)
===========================================================
NOTE: we need python and opencv installed.

  STEP 1:  INSTALLING KERAS   
  ==========================
    METHOD 1:
    ==============
        Installation using PYPI(i.e. using pip)
        .......................................
            sudo pip install keras      # If you are about to install in virtualenvironment, you may want to avoid using sudo

    METHOD 2:
    ==============
        Installation using setup file(i.e. clone from github)
        #navigte to the path you want to put your keras files
        .....................................................
            git clone https://github.com/fchollet/keras.git
            cd keras
            sudo python setup.py install
        

  STEP 2:  SWITCHING KERAS BACKEND(THEANO or TENSORFLOW)   
  ================================
    
    BACKENDS
    ==============
        Keras uses three backends
            1. Tensorflow
            2. Theano
            3. CNTK
        
           TensorFlow is an open-source symbolic tensor manipulation framework developed by Google.
           Theano is an open-source symbolic tensor manipulation framework developed by LISA Lab at Université de Montréal.
           CNTK is an open-source toolkit for deep learning developed by Microsoft.
        
    After installing keras, when we tried to import keras using python it won't import.
    (if we haven't installed tensorfow previously).
     
    you have to find the locasion of .keras folder which contain "keras.json". we will change the keyword for backend column.
    In most of the case keras located in   /home/(user_pc_name)/.keras (user_pc_name is ur pc name, replace
    it with ur user name of pc)
          
          Contents in keras.json file (contents may varies depending upon ur keras versions i.e keras 1.0 or keras 2.0)
          -------------------------------------------------------------------------------------------------------------
           {
              "image_data_format": "channels_last",
              "epsilon": 1e-07,
              "floatx": "float32",
              "backend": "tensorflow"
            }
        
        Using TensorFlow as backend
        ...........................
        |         In order to use tensorflow, we need minimum opencv 3.0 or higher verion of opencv. dont worry about for  
        |           python version, both python 2.7 and python 3 or higher versions support tensorflow.
        |         
        |         To install TensorFlow, check this out https://www.tensorflow.org/install/install_linux
        |         
        |            step 1: open "keras.json" file
        |            step 2: change the backend as "tensorflow"
        |            step 3: save the file


        Using Theano as backend
        ...........................
        |         Theano is directly available. we dont need to install theano.
        |         
        |            step 1: open "keras.json" file
        |            step 2: change the backend as "theano"
        |            step 3: save the file
          
          
   Finaly, import keras in python depending on your backend, python will import keras. 
If you dont get any errors, you have installed keras successfully.
