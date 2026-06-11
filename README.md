# Edwards TIC Instrument Controller 6-Gauge - Tango Device Server

This repository contains the driver to control a TIC Instrument Controller 6-Gauge with the Tango Control.

> **NOTE**: This Device Server is still under development. For now the repository only contains the notebook `Serial_Pressure.ipynb` that tests the serial communication with the controller. The Tango class has not been implemented yet.

## Serial communication test

The notebook [`Serial_Pressure.ipynb`](Serial_Pressure.ipynb) shows how to read a pressure gauge of the TIC controller over the serial (COM) port using `pyserial`:

```python
import serial

serial_port = 'COM9'
TIC = serial.Serial(serial_port, baudrate=9600)

def ReadGauge2():
    cmd = b'?V914\r'
    TIC.flush()
    TIC.write(cmd)
    TIC.flush()
    msg = TIC.read_until(b'\r')
    return msg.decode('utf-8')

print(ReadGauge2())
TIC.close()
```

The command `?V914` asks for the value of the gauge 2. The TIC serial protocol uses commands of the form `?V9xx\r`, where `xx` selects the object to read (gauges 1 to 6 correspond to objects 913 to 918, see the TIC Instruction Manual for the complete list).

# References

- [Edwards TIC Instrument Controller (Turbo and Instrument Controller)](https://www.edwardsvacuum.com/)
- TIC Instruction Manual (D397-22-880) - describes the RS232 serial command protocol.
