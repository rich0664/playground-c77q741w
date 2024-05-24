# Welcome!

This C template lets you get started quickly with a simple one-page playground.

```C runnable
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <stdbool.h>
#include <math.h>

/**
 * Auto-generated code below aims at helping you parse
 * the standard input according to the problem statement.
 **/

 typedef struct vector2 {
    double x;
    double y;
 } vector2;

typedef struct node {
    vector2 pos;
    vector2 a;
    vector2 b;
} node;

 #define CHECKPOINTS_SIZE 10
    vector2 checkpoints[CHECKPOINTS_SIZE];
    uint checkpointAmount=0;
    bool circuitKnown=false;
    bool nodesInitialized = false;

    node checkpointNodes[CHECKPOINTS_SIZE];
    vector2 checkpointswithNodes[CHECKPOINTS_SIZE*3];
    int nodetarget = 0;
    

int wrap(int i,int amount)
{
    while(i<0) return i+amount;
    return i % amount;
}

int updateCheckpoints(vector2 pos)
{
    for(int i=0;i<CHECKPOINTS_SIZE;i++){
        if(pos.x == checkpoints[i].x && pos.y ==checkpoints[i].y){
            if(checkpointAmount>1 && i==0) circuitKnown=true;
            return i;
        }
        else if(checkpoints[i].x==0){
            checkpoints[i].x=pos.x;
            checkpoints[i].y=pos.y;
            checkpointAmount=i+1;
            return -2;
        }
    }
    return -1;
}    


vector2 add(vector2 a, vector2 b)
{
    vector2 c = {a.x + b.x, a.y + b.y};
    return c;
}

vector2 sub(vector2 a, vector2 b)
{
    vector2 c = {a.x - b.x, a.y - b.y};
    return c;
}

vector2 scalar(vector2 a, float f)
{
    vector2 b = {a.x*f,a.y*f};
    return b;
}

vector2 lerp(vector2 a, vector2 b, float t)
{   a = scalar(a,1-t);
    b = scalar(a,t);
    vector2 c = add(a,b);
    return c;
}

double calculateAngle(vector2 a, vector2 b, vector2 c) 
{

    double result = 
        atan2(c.y - a.y, c.x - a.x) - 
        atan2(b.y - a.y, b.x - a.x);
    return result;
}
double distance(vector2 a, vector2 b)
{
    return sqrt(pow((a.x-b.x),2) + pow((a.y-b.y),2));
}
vector2 direction(vector2 a, vector2 b, bool normalize)
{
    vector2 direction = sub(b,a);
    double magnitude = sqrt(direction.x * direction.x + direction.y * direction.y);
    if(normalize){
    direction.x /= magnitude;
    direction.y /= magnitude;
    }
    return direction;
}

vector2 pointInDistance(vector2 a, vector2 direction, double distance)
{
    vector2 b;
    b.x = a.x + direction.x * distance;
    b.y = a.y + direction.y * distance;
    return b;
}


node tangentDir(vector2 a, vector2 b, vector2 c)
{
    vector2 forward = sub(direction(b,c,true),direction(b,a,true));
    vector2 hf = pointInDistance(b,forward,distance(a,b)*0.5);
    vector2 hb = pointInDistance(b,forward,distance(a,b)*-0.5);
    node node;
    return node;
}




double angle(vector2 a, vector2 b, vector2 c)
{
    vector2 AB = direction(a,b,false);
    vector2 BC = direction(b,c,false);
    double dot=    AB.x * BC.x + AB.y * BC.y;
    double pcross= AB.x * BC.y - AB.y * BC.x;
    double angle= atan2(pcross, dot);

    return angle;//*(180.0*M_PI);
}

vector2 rotate90(vector2 a,bool clockwise)
{
    vector2 b = {a.y,a.x};
    if(clockwise) b.y=-b.y;
    else b.x = -b.x;
    return b;

        
}
vector2 velocityTarget(vector2 position, vector2 direction, float speed, float multiplier)
{
    return pointInDistance(position,direction,speed*multiplier);
}

float closeEnough(vector2 position, vector2 target, vector2 expected, bool overshoot)
{
    float a =  distance(target,expected);
    if(overshoot) return distance(position,expected) - distance(position,target) ;
    if(angle(target,position,expected)<0) a = -a;
    return a;
}





double clamp(double d, double min, double max) 
{
  const double t = d < min ? min : d;
  return t > max ? max : t;
}


int main() {
	printf("Hello World!");
}

```

# Advanced usage

If you want a more complex example (external libraries, viewers...), see the [official documentation](https://tech.io/playgrounds/408/tech-io-documentation).
