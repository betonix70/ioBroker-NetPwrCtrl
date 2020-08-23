# ioBroker-NetPwrCtrl
IOT connection of Anel NetPwrCtrl devices

I don't like to code, why I did not choose to develop the usual adapter in ioBroker but use a Node-RED IOT connection instead. ioBroker with its graphical development capabilities is just perfect for this. As I know that others are also looking to implement a solution for Anel devices, I wanted to share my work as a jump start.

I have two Anel devices; "NET-PwrCtrl IO" and "NET-PwrCtrl HUT", each with 8 IO channels and 8 power outlets, no sensors though. I created a data structure for both of them for upload into the ioBroker Object structure (file: javascript.0.Anel.json) and also two Node-RED flows that bind the devices to the data structure (files: NetPwrCtrl_UDP_Input_Distribution.json, NetPwrCrtl_UDP_Outlet_Switch.json). 

Here is what you need to do:

1) Set your UDP ports on the NET-PwrCtrl devices web page for settings to 7577 for Send and 7575 for Receive, allow UDP communication and save the settings.
2) In user settings set your "admin" password to the default, which is "anel", toggle on "no authentication / login" and save the settings. The security concept under UDP is redicoulous, nothing else works for me then using "admin" with no login required. Password for admin can be changed, but then there need to be additional changes in the Node-RED flow.
3) Upload javascript.0.Anel.json under ioBroker Objects which should then look similar to the screen dump in "javascript Data Structure.JPG". 
4) Import "NetPwrCtrl_UDP_Input_Distribution.json" which should then look similar to "IOT-Anbindung ioBroker.JPG" (don't be confused by some German language!)
3) Open the debug tab (rhs) and execute the "wer da ?" trigger (lhs). This gives you the MAC adresses of your device(s) (blackened). Take these and double click on the "Catch MAC" node. Enter your MAC adresses there (screen dump: Use own MAC Addresses.JPG).
5) Execute the "wer da ?" trigger (lhs) again, go back to Objects and check that your data structure is initially filled now (Filled Data Structure.JPG).
6) Each time you use the devices web interface to switch on and off power outlets, the full data set is sent to the out going UDP port and cought by ioBroker to update the data structure.
7) To switch an outlet from ioBroker, you first have to configure your second flow under Node-RED (NetPwrCrtl_UDP_Outlet_Switch). The screen dump "UDP Out.JPG" shows you where to put the IP Adress of your device.
8) Go to Objects and find the "Switch" field for your Outlet (in my case Outlet_1 with "Name: Nr.1") (screen dump: Switch Outlet.JPG). Switch to "yes(true)".
9) Two things will happen; the state of the outlet changes "State" to "1" (screen dump Effect 2.JPG) and the web switches from OFF (Effect 1.JPG) to ON (Effect 3).JPG and back if you choose "no(false)".

From here you can i.e. use Blockly to do whatever you want if an object changes, that's up to you.
