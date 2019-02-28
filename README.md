artdaq_run_control_gui

Installation proccess:
- on sbnd-daq00:
	source /grid/fermiapp/products/artdaq/setup  
- 	qmake -o Makefile ARTDAQ_console.pro 
- 	make

add these lines to daqinterface.py located at your local intallation of 
artdaq-daqinterface ($ARTDAQ_DAQINTERFACE_DIR/rc/control/daqinterface.py):

after "import os":

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

after "import sys":

sys.path.append( os.getcwd() )
sys.stdout = Unbuffered(sys.stdout)
 
