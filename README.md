# Digital-Piano
Bilkent University
Alba Mustafaj

The main feature of the project is sound producing by giving the input to the keyboard
of BETI board and displaying the sound according to the pertaining frequency in the 
audio module. An amplifier is used for a clear sound. The other features are :
  -Frequency  and note's letter display in 7-segment of Basys3.
  -Displaying a piano-like structure in Basys3 lights.
  -Vizual representation of the notes' frequency in RGB matrix of BETI board.
Devices: Basys3, BETI Board
Coding: SystemVerilog
Every module's function and its interaction with the other modules is described below.

top_module
- Function: Top module controls and instantiates all the other modules of the project.
- Timing constrains: 100Mhz clock of basys3 board.
- Description: This module as a whole combines all the other modules of the project. 
  It also makes the interaction with the constraints I/O logics. It contains internal
  logics such as [3:0] in3, in2, in1, in0 which take values from the keyboard module and
  will be used in the displayController and speakerModule. The interaction of all the 
  instantiated modules will give a value to the logic audio, which is later used to generate 
  our final soundBit.

keyboard
- Function: This module translates the button press of 4x4 Button Matrix 
  on Beti Board into proper signals which we will be needed in other modules
- Timing constrains: We put a 20bit counter to divide the basys3 clock and
  generate one with a frequency about 40Hz.
- Description: This module takes input from the Beti board keyboard and generates
  several outputs. Concretely, a 4 bit value which indicates the location of the button
  pressed is passed to the top module. Secondly an 8 bit value for the leds to be
  lighted is passed also to the top module. Additionally, three 4 bit inputs for the 
  7-segment digits passes to the top module as well.

speakerModule
- Function: Based on the input that comes from the top module, it generates the 
  note frequency and sends it to the amplifier in order to produce a soundBit.
- Timing constrains: A 18 bit counter in the clk_shrink module divides the basys3 clock.
- Description: This module takes a 16 bit input from top module which represents each notes' 
  encodings for the segments ( i.e A221, which is the name and the frequency of the note). 
  This input specifies the corresponding sound for the note to be played. According to the input
  the output will be assigned to the wave length of that note. The wave length of each note is 
  defined with the following formula: 100000000/ frequency /2.

segmentController
-Function: This module displays the name and the frequency of the note being played by the user 
  in Beti board keyboard.
- Timing constrains: 100Mhz clock of basys 3 board.
- Description: This module takes four inputs 4 bit each from top module for the digits to be displayed
   in 7-seg-display. It does the encodings for each number and letter and according to the input it gets, 
  displays the corresponding note name and frequency. ( starting from left to right: 
  [name of the note][hundreds] [tens][units]).

rgb_display
- Function: This module displays the frequency of the note being played in 8x8 rgb matrix.
- Timing constrains: clk_converter module divides the 100Mhz clock and still obtains a really fast clock
   which is divided again by 24(decimal) in the rgb module and is obtained a clock with frequency which 
  is approximately equal to basys 3's built in 100Mhz clock.
- Description: For each octave there is a column to which it corresponds. Concretely, if the first note
  of the octave is pressed only the first light of the first column is turned on. A 6 bit input is passed
  from the top module to this module to indicate the note that is being played. It takes a 3 bit input from 
  the top module which indicates the rows that will be lightened. Furthermore, a 3 bit output corresponds to 
  the column which will be lightened (since we only use 3 columns for our 3 octaves) depending on the note. 
  There is also an inner 24 bit logic which controls the colors of lights to be turned on ( we have chosen blue,
  green and purple) in each column. The above mentioned high frequency of the clock is necessary in order for the
  consecutive lights to turn on rapidly without the previous one turning off, so that the visualization is better.
