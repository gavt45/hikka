#hikka — IP camera bruteforcer based on Hikvision SDK.

---

## Mod
Made workaround patch to hikka.go to avoid linux build errors. (i.e 
```# command-line-arguments
cgo-gcc-prolog: In function ‘_cgo_1234567890_Cfunc_NET_DVR_CaptureJPEGPicture’:
cgo-gcc-prolog:44:47: warning: passing argument 3 of ‘NET_DVR_CaptureJPEGPicture’ from incompatible pointer type [-Wincompatible-pointer-types]
In file included from src/hikka.go:4:0:
hikka/include/HCNetSDK.h:13330:28: note: expected ‘LPNET_DVR_JPEGPARA {aka struct <anonymous> *}’ but argument is of type ‘struct <anonymous> *’
 NET_DVR_API BOOL __stdcall NET_DVR_CaptureJPEGPicture(LONG lUserID, LONG lChannel, LPNET_DVR_JPEGPARA lpJpegPara, const char *sPicFileName);
                            ^~~~~~~~~~~~~~~~~~~~~~~~~~
cgo-gcc-prolog: In function ‘_cgo_18b01f590f00_Cfunc_NET_DVR_Login’:
cgo-gcc-prolog:143:48: warning: passing argument 5 of ‘NET_DVR_Login’ from incompatible pointer type [-Wincompatible-pointer-types]
In file included from src/hikka.go:4:0:
hikka/include/HCNetSDK.h:13117:28: note: expected ‘LPNET_DVR_DEVICEINFO {aka struct <anonymous> *}’ but argument is of type ‘struct <anonymous> *’
 NET_DVR_API LONG __stdcall NET_DVR_Login(const char *sDVRIP, const WORD wDVRPort, const char *sUserName, const char *sPassword, LPNET_DVR_DEVICEINFO lpDeviceInfo);
                            ^~~~~~~~~~~~~
# command-line-arguments
src/hikka.go:109: cannot use _Ctype_LPNET_DVR_JPEGPARA(unsafe.Pointer(&imgParams)) (type _Ctype_LPNET_DVR_JPEGPARA) as type *_Ctype_struct___7 in argument to _Cfunc_NET_DVR_CaptureJPEGPicture
src/hikka.go:201: cannot use _Ctype_LPNET_DVR_DEVICEINFO(unsafe.Pointer(&device)) (type _Ctype_LPNET_DVR_DEVICEINFO) as type *_Ctype_struct___6 in argument to _Cfunc_NET_DVR_Login
make: *** [linux] Error 2
``` 
)

## Building

I'm using a makefile, so you should be able to build it under Linux using this command:

    make linux


You can also build it for Windows if you have a MinGW installed:

    make windows

    
And you can make binaries for Linux and Windows by omiting a make target (it is useful for me as I distribute every build to people who don't know anything about compilers):

    make


And now you have a `build` directory with compiled app.

---

## How to compile:

    apt install git golang-go mingw-w64
    git clone https://github.com/superhacker777/hikka
    export GOPATH=$HOME/go
    go get https://github.com/fatih/color
    cd hikka && make

## Usage

You can define some options:

* __-logins, -passwords__ — files where your logins and passwords are stored; it looks for «logins» and «passwords» by default;
* __-check__ — very useful, but still experimental option which allows to check a host before trying to log in (I did some reverse-engineering and I'm not really sure if everything is OK);
* __-shoots__ — a directory where pictures from cameras will be stored; it doesn't download pictures by default;
* __-threads__ — there's no multithreading until you define in how many threads you want to do a job;
* __-csv__ — write results to CSV file.


__Please note that it is hardcoded to read IPs from file called “hosts”!__

A typical cammand is:

    hikka -threads 200 -check -shoots pics/


Here you go, kid.

---

## TODO

1. Export result to JSON, iVMS-compatible CSV and m3u playlist.
2. Some features like checking PTZ- and microphone-enabled cameras.
3. Rewrite bruteforcing routine to make it possible to bruteforce a single camera in multiple threads (there's a one thread for every camera now).

---

## Bugs

There's some bugs in SDK library (memory leaks and cycling that can fuck up everything) and I can't do anything with it, but all in all it isn't that bad.

---

## Contributing

I'm a newbie in Go and this is my first Go program, so the code and some practices may be ugly. Please tell me if you'll find something that I did in wrong way.

Feel free to contribute, yeah.
