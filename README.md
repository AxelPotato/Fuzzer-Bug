### Description
This script is a fork of benaridans fuzzy script with some changed flags and added flexability.
This is a network service fuzzer that also supports binary protocols.
The fuzzer expects to get a sample of a typical payload in hex stream format like the one wireshark outputs (see example), then it sends fuzzing requests to the specified host and port.

### Usage
usage: Fuzzer-Bug.py 

                     [-h] -d DESTINATION -p PORT -f FILENAME [-t TIMEOUT]
                     [-a ATTACK_TECHNIQUE] [-v] [-b PAD_BYTE]
                     [-m MAX_PADDING_LENGTH] [-q PAD_START] [-u]
                     [-r BIT_RANGE]

Arguments:

    -h,                     --help                
                              show this help message and exit
  
    -d DESTINATION,         --destination DESTINATION
                              destination host address
                        
    -p PORT,                --port PORT  
                              destination port
                        
    -f FILENAME,            --filename FILENAME 
                              Input file
                            
    -t TIMEOUT,             --timeout TIMEOUT 
                              Time between requests in seconds
                        
    -a ATTACK_TECHNIQUE,    --attack_technique ATTACK_TECHNIQUE
                              Attack techniques types:
                              0 = original payloads only,
                              1 = byte switch,
                              2 = length fuzzer
                        
    -v, --verbose             set verbosity
  
    -b PAD_BYTE,            --pad_byte PAD_BYTE
                              fuzz using this pad byte
                        
    -m MAX_PADDING_LENGTH,  --max_padding_length MAX_PADDING_LENGTH
                              max size for length fuzz
                        
    -q PAD_START,           --pad_start PAD_START
                              Start fuzzing after this byte
                        
    -u,                     --set_udp         
                              send UDP traffic default is TCP
  
    -r BIT_RANGE,           --bit_range BIT_RANGE
                              Set the Min and Max bit range to fuzz default is 0-255
                        

### Attack techniques
Supported attack techniques (specified using the -a argument):

    0 - orignal payload only  -  just sends the original payload from the file.
    1 - byte switcher         -  Goes over the payload byte by byte and replaces each byte with the default values of 0-255.
    2 - length fuzzer         -  Adds an increasing number of a bytes to the end of the payload.


### Examples
    python Fuzzer-Bug.py -d 127.0.0.1 -p 8080 -t 0.5 -f example.txt -a 1 -v -q 15-77
Sends TCP requests to 127.0.0.1 port 8080 every 0.5 seconds using the byte switcher technique fuzzing only the values between 15 and 77 with a verbose output.

    python Fuzzer-Bug.py -d 127.0.0.1 -p 8080 -t 0.01 -f example.txt -a 2 -v -b 65 -m 10000 -u
Sends UDP requests to 127.0.0.1 port 8080 every 0.01 seconds using the length fuzzer technique padding it with a 65 byte at the end.
