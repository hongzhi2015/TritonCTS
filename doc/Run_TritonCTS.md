# Run TritonCTS

- Change directory to the repository top folder "TritonCTS".

- Make sure you have files _src/tech/lut-XX.txt_ and _/src/tech/sol_list-XX.txt_, where _XX_ is your target enablement. Refer to the [Library Characterization]() tutorial otherwise. TritonCTS library characterization has been validated on ST28 and TSMC16, which would correspond respectively to XX = 28, 16.

- Compile the source code:
``` bash
$ make CPLEXDIR=path/to/cplex/include CONCERTDIR=path/to/concert/include CPLEXLIBDIR=path/to/cplex/static_bin CONCERTLIBDIR=path/to/concert/static_bin
```

- Create a file named _.config_ to specify the input parameters. See an example and description below:

## Example of a _.config_ file
``` tcl
lef /path/to/lef
path /path/to/def
verilog /path/to/verilog
design my_design
target_skew 150
width 140.736
height 140.544
num_sinks 256
clkx 69.09 
clky 140.544
gcellw 1152
gcellh 1152
tech 16
enable_pd 1
```

- Parameters description:
    - _lef_, _path_ and _verilog_ are system paths to the input files;
    - _design_ is a string value specifying the name of the design top module in DEF and Verilog;
    - _target_skew_ is an integer value specifying the target skew in ps;
    - _width_ and _height_ are float values specifying the core dimensions in um;
    - _num_sinks_ is an integer value specifying the number of sink regions;
    - _clkx_ and _clky_ are float values specifying the clock entry point in _um_;
    - _gcellw_ and _gcellh_ are integer values specifying the GCELL sizes for routing in DEF database units;
    - _tech_ is an integer number specifying the technology node. The values 16 and 28 are currently available, corresponding to TSMC16 and ST28, respectively;
    - _enable_pd_ is a boolean value. TritonCTS uses [PD-Rev](https://github.com/abk-openroad/PD-Rev) for routing the leaf-level if enable_pd is set to 1. Otherwise, TritonCTS uses [RST-T](https://dl.acm.org/citation.cfm?id=505366).

- Add a "Dummy buffer" macro to your technology .lef file. You may do this by duplicating any buffer macro and renaming it as "DUMMY".

- Run TritonCTS:
``` bash
$ ./run.sh
```

- A folder named as _DESIGN_TSKEW_NODE_ will be created, where _DESIGN_ is the name of your design; _TSKEW_ is the target skew and _NODE_ is the technology node -- E.g.,  _jpeg_encoder_150_28_. In this folder you may find the output files: final.v, cts_final.def and cts.guides.