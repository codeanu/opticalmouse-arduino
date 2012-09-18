#include <ps2.h>


PS2 mouse(6, 5);


void mouse_init()

{

  mouse.write(0xff);  // reset
  mouse.read();  // ack byte
  mouse.read();  // blank */
  mouse.read();  // blank */
  mouse.write(0xf0);  // remote mode
  mouse.read();  // ack
  delayMicroseconds(100);

}



void setup()

{

  Serial.begin(9600);

  mouse_init();

}



/*

 * get a reading from the mouse and report it back to the

 * host via the serial line.

 */

void loop()

{

  char mstat;

  char mx;

  char my;



  /* get a reading from the mouse */

  mouse.write(0xeb);  // give me data!

  mouse.read();      // ignore ack

  mstat = mouse.read();

  mx = mouse.read();

  my = mouse.read();



  /* send the data back up */

  Serial.print(mstat, BIN);

  Serial.print("\tX=");

  Serial.print(mx, DEC);

  Serial.print("\tY=");

  Serial.print(my, DEC);

  Serial.println();

  delay(100);  /* twiddle */

}