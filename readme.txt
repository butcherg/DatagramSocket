DatagramSocket: C++ Class for UDP communications for Windows/Unix

Glenn Butcher
8/27/2010

Usage:

__________________________________________

#include "DatagramSocket.h"
#include "Thread.h"  //another project
#include <iostream>
#include <string>

using namespace std;

class MyThread: public Thread
{
    private:
        DatagramSocket *s;

    public:
	MyThread(DatagramSocket *sock)
	{
	    s = sock;
		Thread::CreateNewThread(this);
	}

	void Run(void*)
	{
	    char msg[4000];
		while (1) {
		    s->receive(msg, 4000);
		    cout << msg << endl;
		}
	}

};


int main()
{
    DatagramSocket *s = new DatagramSocket(5000, "255.255.255.255", TRUE, TRUE);
	MyThread *t = new MyThread(s);

	string msg = "";

	while (1)  {
	    getline(cin, msg);
	    s->send(msg.c_str(), msg.length());
	}
}

__________________________________________


DatagramSocket collects the more vexing parts of getting UDP communications
to work in a class that is fairly simple to use.  The above example 
uses another Windows/Unix agnostic class I've written, Thread, to implement 
a simple chat client.  Not illustrated is the use of GetAddress() to resolve 
hostnames into IP addresses.

Compiling with CodeBlocks/MinGW:
I tested it with a standard console application, you need to add wsock32 to 
the linked libraries.

Compiling with g++ on Debian:
While not necessary for DatagramSocket itself, to compile the above example 
you need to include -pthread in the command line.
