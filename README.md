artdaq_run_control_gui

run the GUI:
#DAQInterface must be configured(all artdaq environment variables setuped and source_me sourced)
./ARTDAQ_console

build from source code proccess:  
-qmake -o Makefile ARTDAQ_console.pro 
-make

IMPORTANT:
add these lines to daqinterface.py located at your local installation of 
artdaq-daqinterface ($ARTDAQ_DAQINTERFACE_DIR/rc/control/daqinterface.py):

#after "import os":

class Unbuffered(object):
   def __init__(self, stream):
       self.stream = stream
   def write(self, data):
       self.stream.write(data)
       self.stream.flush()
   def writelines(self, datas):
       self.stream.writelines(datas)
       self.stream.flush()
   def __getattr__(self, attr):
       return getattr(self.stream, attr)

#after "import sys":

sys.path.append( os.getcwd() )
sys.stdout = Unbuffered(sys.stdout)
 
