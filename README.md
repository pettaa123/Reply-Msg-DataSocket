# Reply-Msg-DataSocket

Reply Msg for Actor Framework utilizing DataSocket for communication between different machines.

**Note:** LabVIEW's DataSocket Server must be running on one of the machines, preferably the host machine. The tests utilize a modified version of the Network Stream Strategy from the Network Endpoint Actors, which is necessary if your host machine has multiple network adapters. You can find that modified version [here](https://github.com/pettaa123/Network-Stream-Strategy-for-multiple-Network-Interfaces).

**Performance:** Tests showed a reply time of <4 ms between a Windows host and a cRIO 9045 running RT Linux. However, the very first reply time may be several hundred milliseconds. Consider a single dummy read before your time critical process starts.

## Usage

1. Add `Reply Msg DataSocket.lvclass` to your LabVIEW project.
2. Create a message with an output indicator on the target machine using the right-click menu: **Actor Framework -> Create Message**.
3. Change the inheritance of the newly created Msg Object to `Reply Msg DataSocket.lvclass`.
4. Override the `Do Core` method.
5. Copy the content of `Do.vi` to `Do Core.vi`.
6. You can delete `Do.vi` now.
7. In your `Send <your message name>.vi`, replace `Enqueue.vi` with `Send Message and Wait for Response.vi` from `Reply Msg DataSocket.lvclass`.
8. Adjust the connector pane of `Send <your message name>.vi`.

You can refer to `Reply Msg DataSocket.vi` in `/Tests/Target Reply Time Test Actor.lvclass`.

**Important:** Since connections are managed in an Action Engine, make sure to call the Action Engine in `DataSocket Read Connections Manager.vi` or `DataSocket Write Connections Manager.vi` with the action `Clear all` between consecutive runs. Call `DataSocket Read Connections Manager.vi` if you are on the machine hosting the DataSocket Server; call `DataSocket Write Connections Manager.vi` when you are on the target machine (not hosting the DataSocket server).

For example, in `Pre Launch Init.vi`.