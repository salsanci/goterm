# Go Term
This is a golang repository that provides a rich but specialized terminal, primarily aimed at supported embedded development.

# Command Line Parameters
* `-h`                      - print help information
* `-afc`                    - Uses Afc flow control
* `-baud`                   - int Baud rate (default 115200)
* `-cr`                     - Expand CR to CR LF to console
* `-crc`                    - Can use crc for multiplexed channels, no effect otherwise
* `-d`                      - Debug mode
* `-dataPrefix`             - string - Data file prefix
* `-host`                   - string - IP host
* `-ipToSerial`             - Route ip to serial port
* `-l`                      - Loopback mode
* `-lf`                     - Expand LF to CR LF from console
* `-lineDelay`              - int - Send line delay in milliseconds
* `-log`                    - Log to file
* `-logDir`                 - string  - Logging directory (default "./")
* `-logHours`               - int - Log hours per file (default 24)
* `-muxHost`                - string - IP multiplexor host
* `-muxPort`                - int - Multiplexor port (default 2001)
* `-numMux`                 - int - Number of multiplexor channels 0, or 2-16 (default -1)
* `-p`                      - string - Profile Name from .goterm.json, search current directory and the home directory
* `-phi`                    - Priority High
* `-port`                   - int - Port (default -1)
* `-prefix`                 - string - Log file prefix
* `-remoteHost`             - string - Connect to this host and remotePort for console via tcp/ip
* `-remotePort`             - int - Remote port, if remote host is not specified console will be available on this port via tcp/ip
* `-serial`                 - string - Com port for serial communications, Example: "/dev/serial1"
* `-serialResetTimeout`     - int - In milliseconds, reset the serial port if nothing received for n ms
* `-suppressBlankLineDelay` - Suppress blank line delay, whether to suppress lineDelayMs (default true)
* `-ts`                     - int - Timestamp interval (minutes) (default -1)
* `-udp`                    - Use udp instead of tcp
* `-useFastMux`             - Uses the fast multiplexor
* `-x`                      - Filter hex characters to console
* `-xbox`                   - Enable Xbox controller
* `-xboxDebug`              - Enable Xbox controller debug

## Examples
* `goterm -serial com9 -baud 230400`  - use goterm with a serial port
* `goterm -host localhost -port 23` - use goterm to connect to a telnet port
* `goterm -port 23` - use goterm listen on port 23
* `goterm -serial com9 -baud 230400 -numMux 2 -muxport 2020` - use goterm with a serial port, start muxserver on ports 2020 - 2021
* `goterm -port 23 -numMux 2 -muxhost localhost -muxport 2020` - use goterm to listen on port 23, connect to ports 2020 - 2021 on localhost

## Profiles
Profiles are loaded from a json file.  The json file can be in the current directory or the home directory.

See [Example JSON](goterm/.goterm.json)

# Terminal Commands
These commands can be used while goterm is running.

* `--help`                      - print help information
* `--debugOn`                   - turns on debugging
* `--debugOff`                  - turns off debugging
* `--ipDebugOn`                 - turns on debugging for ip channels
* `--ipDebugOff`                - turns off debugging for ip channels
* `--serDebugOn`                - turns on debugging for serial channels
* `--serDebugOff`               - turns off debugging for serial channels
* `--testDebugOn`               - turns on debugging for mux channels
* `--testDebugOff`              - turns on debugging for mux channels
* `--speedTestEnable`           - enables goterm acting as a speedTest receiver
* `--speedTestDisable`          - disables goterm acting as a speedTest  receiver
* `--speedReceiveEnable`        - enables goterm acting as a speedRecieve receiver
* `--speedRecieveDisable`       - disables goterm acting as a speedReceive receiver
* `--lineDelayMs`               - the number of milliseconds to delay after a line is sent
* `--delay x`                   - delays input by x milliseconds (default value is 0) 
* `--printTime`                 - prints a timestamp to the console
* `--waitOutputIdle`            - waits for output to the console to be idle for more that 2 seconds
* `--speedTest x y z`           - runs a speed test with x bytes, blocksize of y bytes (max for udp is 1024), block delay z nanoseconds  (defaults are 1000000 1024 100000)
* `--timestamp x`               - writes a timestamp to the terminal every x minutes, 0 disables timestamp (default value is 0) 
* `--dataCaptureEnable`         - enables data capture
* `--dataCapture x...z`         - captures data from the host port and writes to a file, the rest of the line is sent to the host to initiate data capture
* `--dataCaptureTimeout x`      - terminates data capture after x milli-seconds of inactivity (default is 600) 
* `--speedSend x y z`           - runs a speed sending data only with x bytes, blocksize of y bytes (max for udp is 1024),  block delay z nanoseconds  (defaults are 1000000 1024 100000)
* `--sendBaudLimit x`           - limits the tx line speed
* `--tone x y`                  - play a tone, x - freq in hz ( 300 - 7000), y - length in ms ( 100 - 10000) (windows only)


# Multiplexor protocol:
Used to support multiple logical channels to an fpga or micro over a connection.
Up to 16 channels are supported.

## Protocol to / from host:
* [C][N] [XX...ZZ]
* [C] - 4 bits channel number
* [N] - 4 bits number of bytes ( 1 - 15) If crc is specified last 2 bytes are a ccitt crc which include CN
* [XX...ZZ] number of bytes specified by N


# Building
* Use bash script `./build.sh`
    * This will build for Windows, Linux and OSX