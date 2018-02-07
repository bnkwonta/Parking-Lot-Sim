# Parking-Lot-Sim
Faulty. Displays parking lot simulator

ParkingLot mylot = new ParkingLot();
Street mystreet = new Street();
Gate mygate = new Gate();
Date mydate = new Date(1,2,3, true);
ParkingStallSection mysection = new ParkingStallSection(0,0);
Car [] cars1 = new Car[1];
Car [] cars2 = new Car[1];
Car [] cars3 = new Car[1];
Car [] cars4 = new Car[1];
int spaces = 60;
boolean park;
Date myDate;
ControlPanel mypanel = new ControlPanel();
int customers = 0;
float profit = 0;

void setup() {
  // creates parking stall objects
  size (900, 900);
  background(#036203);
  for (int i=0; i<cars1.length; i++) {
    cars1[i] = new Car(-0);
    cars2[i] = new Car(-120);
    cars3[i] = new Car(-240);
    cars4[i] = new Car(-360);
  }
  myDate = new Date(0,1,0,true);
}


void draw() {
  mylot.drawLot();
  mystreet.drawStreet();
  mygate.drawGate();
  mygate.drawSign();
  mypanel.drawPanel();
  myDate.addMinute();
 
  for (int i =0; i<cars1.length; i++) {
    cars1[i].drawCar();
    cars2[i].drawCar();
    cars3[i].drawCar();
    cars4[i].drawCar();
  }
  for (int j=0; j<cars1.length; j++) {
    cars1[j].drivein();

    cars2[j].drive();
    cars3[j].drive();
    cars4[j].drive();
  }
}


Car Class

class Car {
  float sizex = 30;
  float sizey = 20;
  float xposN;
  float yposN = 140;
  float yposS = 635;
  color carc = color(random(0, 120), random(0, 140), random(0, 160));
int speed = 2;
  Car(int x)
  {
    xposN = x;
  }


  void drawCar() {
    fill(carc);
    rect(xposN, yposN, sizex, sizey);
  }


  void drive() {
    if (xposN>= -500 &&  yposN == 140) {
      xposN +=speed;
    }
    if (xposN>= 1000) {
      xposN = -40;
      yposN = 140;
      carc = color(random(0, 120), random(0, 130), random(0, 140));
    }
  }
  void drivein() {

    if (xposN>= -500 && xposN < 436 && yposN == 140 && spaces > 0 && spaces <= 60) {
      xposN +=speed;
    }
    if (xposN == 436 && yposN >= 140) {
      xposN = 436;
      yposN +=1;
    }    


    if ( xposN == 436 && yposN >= 180) {
      yposN +=1;
      park = true;
      spaces -= 1;
      customers++;
    }
if(yposN >= 180 && yposN <= 635){
  xposN += 2;
  yposN = 635;
}

    if (xposN == 1000) {
      xposN = -40;
      yposN = 140;
      carc = color(random(0, 120), random(0, 130), random(0, 140));
      
    
  }
    
  }
  
     
    }
  
  
  
  Control Panel Class
  
  class ControlPanel{

  void drawPanel(){
    fill(0);
    rect(0,0,900,122);

stroke(255);     //Parking Rate Display
strokeWeight(2);
fill(#898989);
rect(8,8,195,112);  

fill(255);
text("Parking Rates:" ,12,25);
text("$3.00 / Hour    Mon - Sat", 12, 45);
text("$1.50 / Hour    Sun", 12, 65);

stroke(255);      //Profits and customers
strokeWeight(2);
fill(#898989);
rect(390,8,195,112);

fill(255);
text("Simulation Values",394, 25); 
text("Profits:  \t$" + profit + "0", 394, 45);
text("Customers: \t" + customers, 394, 65);


stroke(255);      //Date And Time Display
strokeWeight(2);
fill(#898989);
rect(640,8,200,112);

fill(255);
text("Date and Time:  ", 644, 25);
text(myDate.toString(), 644, 45);

}



}


Date Class

class Date {
  final String [] days = {"Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday", "Sunday"};
  int today;
  int hour;
  int minute;
  boolean before_noon;

  Date(int d, int h, int m, boolean beforeNoon) {

    today =d%7;
    if ( h%12 ==0 && before_noon == false && m< 60) {
      hour = h;
    } else {
      hour = h%12;
    }
    minute = m%60;
    before_noon = beforeNoon;
  }
  Date(Date d) {
    today = d.today;
    hour = d.hour;
    minute = d.minute;
    before_noon = d.before_noon;
  }

  void addHour() { 
    if (  hour < 11 ) {
      hour++;
    } else if (hour == 11 && before_noon ==false) {
      before_noon = !before_noon;
      hour++;
      today++;
    } else if (hour == 11 && before_noon == true) {
      before_noon = !before_noon;
      hour++;
    } else if (hour == 12) { 
      hour =1;
    }
    

    if (today==7) {
      today=0;
    }
  }
  void addMinute() {
    minute++;
    if (minute == 60) {
      minute = 0;
      addHour();
    }
  }
  String toString() {
    String date = days[today];

    if (hour <10) {
      date += " 0" + hour;
    } else {
      date += " " + hour;
    }

    if (minute < 10) {
      date += ":0" + minute;
    } else {
      date += ":" + minute;
    }

    if (before_noon) {
      date += "AM";
    } else {
      date += "PM";
    }
    return date;
  }

  boolean equal (Date date) {
    if (date.today == today && date.hour == hour && date.minute == minute && date.before_noon == before_noon) {
      return true;
    } else {
      return false;
    }
  }
}


Gate Class

class Gate {
  float cost;
  boolean entered;
  int gatex1 = 430;
  int gatey1 = 170;
  int gatex2 = 470;
  int gatey2 = 190;
  int open;
  int closed;

  void drawSign() {
    stroke(0);
    strokeWeight(4);
    fill(255);
    rect(475, gatey1, 105, 20);
    rect(475, gatey1 + 410, 105, 20);
    fill(#841BCB);
    rect(475, gatey2, 105, 20);
    rect(475, gatey2+410, 105, 20);
    fill(0);
    textSize(15);
    text("Entrance", 495, gatey1+15);
    text("Spaces = " + spaces, 485, 205);
    text("Exit", 510, gatey2+405);
    text("Fee = TBA", 490, gatey2+425);
    
  }

  void drawGate() {
    stroke(0);
    strokeWeight(7);
    line(gatex1, gatey1, gatex2, gatey2);  // entrance
    line(gatex1, gatey1 + 450, gatex2, gatey2 + 415 ); // exit
    strokeWeight(0);
    fill(#07F218);
    ellipse(gatex1, gatey1, 5, 5);
    ellipse(gatex1, gatey1 + 450, 5, 5);
    if (spaces > 0){
      entered = true;
      
    } else { 
     entered = false;
    }
    if (entered == true){
      gatex2 = gatex1;
      open = closed;
    }
    
  }

}


Parking Lot Class

class ParkingLot {
  ParkingStallSection [][] section = new ParkingStallSection[3][2];
  int lotw = 840;
  int loth = 420;


  ParkingLot () {    // sets parameters for 3x2 lot
    for (int i = 0; i < 3; i++) {
      for (int j = 0; j < 2; j++) {
        section [i][j] = new ParkingStallSection (60+j*405, 215+i*130 );
      }
    }
  }
  void drawLot() {
    stroke (255);
    strokeWeight (2);
    fill (100);
    rect (30, 185, lotw, loth);
    for (int i = 0; i < 3; i++) {
      for (int j = 0; j < 2; j++) {
        section[i][j].drawSection();
      }
    }
  }
}


Stall Section Class


class ParkingStallSection
{
  
 
  ParkingStall [][] secto;

  ParkingStallSection (int x, int y) {
   
    secto = new ParkingStall [5][2];
    int i = 0;
    int j = 0;
    for (i = 0; i < 5; i++) {
      for (j = 0; j < 2; j++) {
        secto [i][j] = new ParkingStall (x+i*75, y+j*50, 75, 50);
      }
    }
  }
  void drawSection() {
    for (int i = 0; i < 5; i++) {
      for (int j = 0; j < 2; j++) {
        secto [i][j].drawStall();
      }
    }
  }
}


Stall Class

class ParkingStall {
  // STALL ATTRIBUTES
  Date timeTaken;
  boolean occupied;
  // DIMENSIONS AND POSITION
  int stallWidth;
  int stallHeight;
  int posX;
  int posY;

  ParkingStall(int x, int y, int w, int h) {
    occupied = false;
    posX = x;
    posY = y;
    stallWidth = w;
    stallHeight = h;
  }

  void drawStall() {
    if (occupied)
      fill(color(255, 90, 71)); // RED STALL
    else
      fill(color(152, 251, 152));  // GREEN STALL
    strokeWeight(4);
    stroke(255);
    rect(posX, posY, stallWidth, stallHeight);
    strokeWeight(1);
  }

  // Sets whether the stall is occupied or not
  void setStatus(boolean status, Date time)
  {
    occupied = status;
    if (occupied)
      timeTaken = new Date(time);
  }


}


Price Calculator Class

class PriceCalculator {

  float rate;
  
  Date enter;
  Date exit;

  PriceCalculator(Date enter, Date exit) {

    this.enter = enter;
    this.exit = exit;
    if (enter.today != 6)
      rate = 3.0;
    else
      rate = 1.5;
  }

  int counter = 0;
  double fee() {

    while (enter.equal(exit) == false) {

      if (counter < 60) {
        enter.addMinute();
        counter++;
      } else if (counter == 60) {
        if (enter.today != 6) {
          rate += 3;
        } else 
        rate += 1.5;
      }
      counter = 0;
      enter.addMinute();
    }
    return rate;
  }
}


Street Class

class Street {
  void drawStreet() {
    stroke(255);
    strokeWeight(2);
    fill(100);
    rect(-5, 125, 905, 45);
    stroke(100);
    strokeWeight(0);
    rect(425, 150, 50, 50);
    fill(255);
    textSize(20);
    text("North Street", 390, 150);

    stroke(255);
    strokeWeight(2);
    fill(100);
    rect(-5, 620, 905, 45);
    stroke(100);
    strokeWeight(0);
    rect(425, 595, 50, 50);
    fill(255);
    textSize(20);
    text("South Street", 390, 645);
  }
}
