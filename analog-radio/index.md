# Analog Radio


# ANALOG RADIO!

being a fan of old analog radio sets, I decided to collect these related projects starting from a Galena radio receiver that does not need power supply to operate, up to some Arduino experiments... Some analog but digitally driven audio transmission and analog modulation (mostly amplitude). Have fun!

The first is an audio transmitter over LW radio band (734 kHz) in amplitude modulation (AM) which might be easily picked by a once-common radio set.

## ARDUINO AM

A little playground to test audio transmission in AM (amplitude modulation) using the Arduino timers to generate the carrier frequency and its internal ADC to modulate the tuned circuit at the antenna side with the amplitude of the input signal.

{{< image src="am_receiver.jpg" caption="AM Receiver schematic" >}}


Here is a simple AM Receiver schematic included for reference and testing, but being the original source, it has to be adapted to the wavelength used in the above project as it is tuned for 88-108MHz or higher (VHF). Extra wire-loops are needed when winding L1 to lower its resonant frequency.

The second experiment is about transmitting encoded data (morse) over the same media.
A third experiment explores the frequency modulation with little success.

### ARDUINO AM SOURCE CODE

```

//  Simple AM Radio Signal Generator :: Markus Gritsch
//  http://www.youtube.com/watch?v=y1EKyQrFJ-o
//
//                /|\ +5V                               ANT
//                 |                                   \ | /
//                 |        ----------------            \|/
//                | | R1   | Arduino 16 MHz |     C2     |
//                | | 47k  |                |     ||     |  about
//  audio   C1    | |      |      TIMER_PIN >-----||-----+  40Vpp
//  input   ||     |       |                |     ||     |
//    o-----||-----+-------> INPUT_PIN      |     1nF    |
//         +||     |       |                |             )
//          1uF   | | R2   |   ATmega328P   |             ) L1
//                | | 47k   ----------------   fres =     ) 47uH
//    fg < 7 Hz   | |                          734 kHz    )
//                 |                                     |
//                 |                                     |
//                --- GND                               --- GND
//
//    fg = 1 / ( 2 * pi * ( R1 || R2 ) * C1 ) < 7 Hz
//  fres = 1 / ( 2 * pi * sqrt( L1 * C2 ) ) = 734 kHz


#define INPUT_PIN  0 // ADC input pin
#define TIMER_PIN  3 // PWM output pin, OC2B (PD3)
#define DEBUG_PIN  2 // to measure the sampling frequency
#define LED_PIN   13 // displays input overdrive

#define SHIFT_BY   3 // 2 ... 7 input attenuator
#define TIMER_TOP 20 // determines the carrier frequency
#define A_MAX TIMER_TOP / 4

void setup() {
    pinMode( DEBUG_PIN, OUTPUT );
    pinMode( TIMER_PIN, OUTPUT );
    pinMode( LED_PIN, OUTPUT );

    // set ADC prescaler to 16 to decrease conversion time (0b100)
    ADCSRA = ( ADCSRA | _BV( ADPS2 ) ) & ~( _BV( ADPS1 ) | _BV( ADPS0 ) );

    // non-inverting; fast PWM with TOP; no prescaling
    TCCR2A = 0b10100011; // COM2A1 COM2A0 COM2B1 COM2B0 - - WGM21 WGM20
    TCCR2B = 0b00001001; // FOC2A FOC2B - - WGM22 CS22 CS21 CS20

    // 16E6 / ( OCR2A + 1 ) = 762 kHz @ TIMER_TOP = 20
    OCR2A = TIMER_TOP; //   = 727 kHz @ TIMER_TOP = 21
    OCR2B = TIMER_TOP / 2; // maximum carrier amplitude at 50% duty cycle
}

void loop() {
    // about 34 kHz sampling frequency
    digitalWrite( DEBUG_PIN, HIGH );
    int8_t value = ( analogRead( INPUT_PIN ) >> SHIFT_BY ) -
                   ( 1 << ( 9 - SHIFT_BY ) );
    digitalWrite( DEBUG_PIN, LOW );

    // clipping
    if ( value < -A_MAX ) {
        value = -A_MAX;
        digitalWrite( LED_PIN, HIGH );
    } else if ( value > A_MAX ) {
        value = A_MAX;
        digitalWrite( LED_PIN, HIGH );
    } else {
        digitalWrite( LED_PIN, LOW );
    }

    OCR2B = A_MAX + value;
}

```


### ARDUINO AM CW

```


// AM broadcasting CW using arduino
// Based on the code from WWW.JONNYBLANCH.WEBS.COM, which was broken


long millisAtStart=0;
long millisAtEnd=0;

const long period_broadcast=8; //period of shortest broadcast (256 port changes)

#define LENGTH_DIT 50
//number of period_broadcasts in one 'dit',
//all other lengths are scaled from this
//Broadcasts on 1337 kHz

// You should put some piece of wire as antenna in digital port 8. 
// The experiments at www.mateslab.com.ar show us that a 20cm cable can be tuned with a radio from 2,5 meters. With a 50cm antena you can tune it from 5 meters.


const int length_dit=LENGTH_DIT; //number of periods for dit
const int pause_dit=LENGTH_DIT; //pause after dit
const int length_dah=3*LENGTH_DIT; //number of persots for dah
const int pause_dah=LENGTH_DIT; //pause after dah
const int length_pause=7*LENGTH_DIT; //pause between words

void dit(void);
void dah(void);
void pause(void);
void broadcast(int N_cycles);
void dontbroadcast(int N_cycles);

// ### INC ### Increment Register (reg = reg + 1)
#define ASM_INC(reg) asm volatile ("inc %0" : "=r" (reg) : "0" (reg))

void setup()
{

  Serial.begin(9600);
  DDRB=0xFF;  //Port B all outputs
  //Do one dit to determine approximate frequency
  millisAtStart=millis();
  dit();
  millisAtEnd=millis();
  Serial.print(millisAtEnd-millisAtStart);
  Serial.print(" ");
  Serial.print((length_dit+pause_dit)*period_broadcast*256/(millisAtEnd-millisAtStart)/2);
  Serial.print("kHz ");
  Serial.println();
}

void loop()
{

  
// Sends SOS

dah();
char_pause();
dah();
char_pause();
dah();
char_pause();

dit();
char_pause();
dit();
char_pause();
dit();
char_pause();

word_pause();


}



// Outputs a dot (.)
void dit(void)
{
  for(int i=0; i <= length_dit; i++) // now is 64
      broadcast(period_broadcast); 
}

// Output a dash (-)
void dah(void)
{
  for(int i=0; i <= length_dah; i++) // now is 64*3
      broadcast(period_broadcast); 
}


// Short pause between chars
void char_pause(void)
{
  for(int i=0; i <= length_dit; i++) // now is 64
    dontbroadcast(period_broadcast);
}

// Long pause between words
void word_pause(void)
{
  for(int i=0; i <= length_pause; i++) // now is 64
    dontbroadcast(period_broadcast);
}


// This send something to the air
void broadcast(int N_cycles)
{
  unsigned int portvalue;
  for (int i=0;i < N_cycles; i++) {
    do
    {
	PORTB=portvalue;
	ASM_INC(portvalue);
    }
    while(portvalue<255);
  }
}


// This do not send anything to the air
void dontbroadcast(int N_cycles)
{
unsigned int portvalue;
  PORTB=0x00;
  for (int i=0;i < N_cycles; i++) 
  {
    do {
  	ASM_INC(portvalue);
  	//add some assembly No OPerations to keep timing the same
  	asm volatile ("NOP");
      } while(portvalue < 255);
  }    
}

```


## Galena

Galena or lead glance, is the natural mineral form of lead sulfide (PbS) being one of the most abundant and widely distributed sulfide minerals. One of the oldest uses of galena was as an eye cosmetic. Galena is a semiconductor with a small band gap of about 0.4 eV, which found use in early wireless communication systems. It was used as the crystal in radio receivers, in which it was used as a point-contact diode capable of rectifying alternating current to detect the radio signals. The galena crystal was used with a sharp wire, known as [cat's whisker](https://en.wikipedia.org/wiki/Crystal_detector) in contact with it.

### Galena Radio

{{< image src="galena_fm.jpg" caption="Crystal Radio with Galena" >}}

This is a rather funky project, as it powers itself up from the radio waves it receives, but you will need a very high-impendace headset (> 10KΩ)


[Crystal Radio](https://en.wikipedia.org/wiki/Crystal_radio) or Galena Radio are "unpowered" radio-receivers sets. They actually power-up themselves by using the captated radio energy via its Antenna. Some technical consideration (like using high-impedance headphones) reduces the energy-losses and maximize efficiency, in order to have a working radio receiver without any power supply, not batteries nor dinamo.

