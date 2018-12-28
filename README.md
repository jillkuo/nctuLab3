# Route Configuration

This repository is a lab for NCTU course "Introduction to Computer Networks 2018".

---
## Abstract

In this lab, we are going to write a Python program with Ryu SDN framework to build a simple software-defined network and compare the different between two forwarding rules.

---
## Objectives

1. Learn how to build a simple software-defined networking with Ryu SDN framework
2. Learn how to add forwarding rule into each OpenFlow switch

---
## Execution

> TODO:
> * How to run your program?
> * What is the meaning of the executing command (both Mininet and Ryu controller)?
> * Show the screenshot of using iPerf command in Mininet (both `SimpleController.py` and `controller.py`)  

這是這次LAB所使用的拓樸結構圖  
![alt text](https://github.com/nctucn/lab3-jillkuo/blob/master/src/lab3_png/topo_2.jpg)  
以這張圖為條件限制，編輯完topo.py  

開啟兩個終端機來到正確的地址  
第一個終端機先輸入  
> mn --custom topo.py --topo topo --link tc --controller remote  

分別代表的意思是進入Mininet CLI(mn)，指定topo.py(--custom topo.py)，且在這個py檔中，最後一行的名稱為topo(--topo topo)，對link設定參數(--link tc)，Using a Remote Controller(--controller remote)  
若出現錯誤訊息RTNETLINK answers: File exists，則輸入  
> mn -c  

來clean up Mininet，然後再試一次即可  
![alt text](https://github.com/nctucn/lab3-jillkuo/blob/master/src/lab3_png/run_topo.png)  

第二個終端機再輸入  
> ryu-manager SimpleController.py --observe-links  

意思是使用Ryu Controller(ryu-manager PYTHON_FILE_NAME.py --observe-links)，controller的規則為SimpleController.py，若要以另一種規則來進行比較，則將SimpleController.py改寫為controller.py，以另一個py檔來定義新規則  

run SimpleController.py  
![alt text](https://github.com/nctucn/lab3-jillkuo/blob/master/src/lab3_png/run_Simplecontroller.png)  

run controller.py  
![alt text](https://github.com/nctucn/lab3-jillkuo/blob/master/src/lab3_png/run_controller.png)  


接著回到第一個終端機，在CLI mode中輸入iPerf command  
若是使用SimpleController.py則第一行輸入  
> h1 iperf -s -u -i 1 -p 5566 > ./out/result1 &  

若是使用controller.py則第一行輸入  
> h1 iperf -s -u -i 1 -p 5566 > ./out/result2 &  

接下來第二行輸入  
> h2 iperf -c 10.0.0.1 -u -i 1 -p 5566  

意思分別是將名為h1的Host訂為Server(-s)，使用UDP協定(-u)，每經過一秒都顯示一筆數據(-i 1)，結果輸出到./out/result1或是./out/result2，使用的port為5566(-p 5566)  
名為h2的Host訂為Client(-c)，其server的IP為10.0.0.1，使用UDP協定(-u)，每經過一秒都顯示一筆數據(-i 1)，其server使用的port為5566(-p 5566)  

result1  
![alt text](https://github.com/nctucn/lab3-jillkuo/blob/master/src/lab3_png/run_result1.png)  

result2  
![alt text](https://github.com/nctucn/lab3-jillkuo/blob/master/src/lab3_png/run_result2.png)  

---
## Description

### Tasks

> TODO:
> * Describe how you finish this work in detail

1. **Environment Setup**  

打開PieTTY後連線140.113.195.69並login  
若要更改密碼則輸入指令  
> passwd  

接下來是要複製我們這次的lab專案到container中，輸入指令  
> git clone https://github.com/nctucn/lab3-jillkuo.git  

並登入帳號，專案就會下載到root資料夾中，輸入  
> ls  

就會看到名為lab3-jillkuo的資料夾，就是本次lab的專案  
接著要試跑Mininet，輸入  
> mn  

若出現錯誤訊息，輸入  
> service openvswitch-switch start  

即可解決  

2. **Example of Ryu SDN**  

* 先後順序很重要  

一開始先在一個終端機跑SimpleTopo.py，先將路徑改到src資料夾，輸入  
> mn --custom SimpleTopo.py --topo topo --link tc --controller remote  

進入CLI mode  
意思是進入Mininet CLI(mn)，指定SimpleTopo.py(--custom SimpleTopo.py)，且在這個py檔中，最後一行的名稱為topo(--topo topo)，對link設定參數(--link tc)，Using a Remote Controller(--controller remote)  
若出現錯誤訊息RTNETLINK answers: File exists，則輸入  
> mn -c  

來clean up Mininet即可  

再開啟另一個終端機跑SimpleController.py，先將路徑改到src資料夾，輸入  
> ryu-manager SimpleController.py --observelinks  

使用Ryu Controller(ryu-manager PYTHON_FILE_NAME.py --observe-links)，controller的規則為SimpleController.py  
接下來就是要結束執行程式，先在執行SimpleTopo.py的終端機中的CLI mode中輸入  
> exit  

來離開CLI mode，然後再執行SimpleController.py的終端機中按Ctrl加z來中斷程式，然後輸入  
> mn -c  

Clean up Mininet  

3. **Mininet Topology**  

首先要複製SimpleTopo.py到topo.py，輸入  
> cp SimpleTopo.py topo.py  

cp就是複製檔案的指令，SimpleTopo.py是被複製的檔案，topo.py是複製後的檔案名稱  
接著編輯topo.py，根據下圖的限制增加條件，輸入  
> vim topo.py  

進入topo.py，按i開始編輯，按ESC離開編輯模式，輸入  
> :wq  

回到終端機  
![alt text](https://github.com/nctucn/lab3-jillkuo/blob/master/src/lab3_png/topo_2.jpg)  

* 先後順序很重要  

接下來執行程式看有沒有BUG，輸入  
> mn --custom topo.py --topo topo --link tc --controller remote  

進入Mininet CLI(mn)，指定topo.py(--custom topo.py)，且在這個py檔中，最後一行的名稱為topo(--topo topo)，對link設定參數(--link tc)，Using a Remote Controller(--controller remote)  
若出現錯誤訊息invalid syntax 代表topo.py中有BUG  
若出現錯誤訊息File exists，則輸入  
> mn -c  

即可解決  
接下來再另一個終端機的src資料夾路徑中輸入指令  
> ryu-manager SimpleController.py –observe-links  

使用Ryu Controller(ryu-manager PYTHON_FILE_NAME.py --observe-links)，controller的規則為SimpleController.py  

要結束程式的話，跟之前一樣，先在執行topo.py的終端機中的CLI mode中輸入  
> exit  

來離開CLI mode，然後再執行SimpleController.py的終端機中按Ctrl加z來中斷程式，然後輸入  
> mn -c  

Clean up Mininet  

4. **Ryu Controller**  

首先複製SimpleController.py到controller.py中，輸入指令  
> cp SimpleController.py controller.py  

cp是複製檔案的指令，SimpleController.py是被複製的檔案，controller.py是複製後的檔案名稱  
接著編輯controller.py，根據助教給的限制增加條件，輸入  
> vim controller.py  

進入controller.py，按i開始編輯，按ESC離開編輯模式，輸入  
> :wq  

而在controller.py中，我們只需要編輯
    ```python
    switch_features_handler(self, ev)
    ```
這個function即可  

5. **Measurement**  

接著我們要開始做實驗，比較在網路拓圖結構為topo.py的情況下，使用SimpleController.py的forwarding rules和使用controller.py的forwarding rules會有什麼樣的差別  

* 先後順序很重要  

首先執行topo.py，在一個終端機中輸入  
> mn --custom topo.py --topo topo --link tc --controller remote  

進入Mininet CLI(mn)，指定topo.py(--custom topo.py)，且在這個py檔中，最後一行的名稱為topo(--topo topo)，對link設定參數(--link tc)，Using a Remote Controller(--controller remote)  
若出現錯誤訊息File exists，則輸入  
> mn -c  

即可  
接著在另一個終端機中執行SimpleController.py，輸入指令  
> ryu-manager SimpleController.py –observe-links  

使用Ryu Controller(ryu-manager PYTHON_FILE_NAME.py --observe-links)，controller的規則為SimpleController.py  
然後再回到執行topo.py的終端機，在CLI模式中輸入指定的測量指令  
> h1 iperf -s -u -i 1 -p 5566 > ./out/result1 &  

第二行輸入  
> h2 iperf -c 10.0.0.1 -u -i 1 -p 5566  

將名為h1的Host訂為Server(-s)，使用UDP協定(-u)，每經過一秒都顯示一筆數據(-i 1)，結果輸出到./out/result1，使用的port為5566(-p 5566)  
名為h2的Host訂為Client(-c)，其server的IP為10.0.0.1，使用UDP協定(-u)，每經過一秒都顯示一筆數據(-i 1)，其server使用的port為5566(-p 5566)  
然後要結束執行程式，在執行topo.py的終端機中的CLI模式中輸入  
> exit  

在執行SimpleController.py的終端機中按Ctrl加z直接中斷程式，然後輸入  
> mn -c  

Clean up Mininet  

然後再接著測量controller.py  
再執行一次topo.py，在一個終端機中輸入  
> mn --custom topo.py --topo topo --link tc --controller remote  

進入Mininet CLI(mn)，指定topo.py(--custom topo.py)，且在這個py檔中，最後一行的名稱為topo(--topo topo)，對link設定參數(--link tc)，Using a Remote Controller(--controller remote)  
若出現錯誤訊息File exists，則輸入  
> mn -c  

即可  

接著在另一個終端機中執行controller.py，輸入指令  
> ryu-manager controller.py –observe-links  

使用Ryu Controller(ryu-manager PYTHON_FILE_NAME.py --observe-links)，controller的規則為controller.py  
若出現錯誤訊息ImportError: No module named controller.py則表示controller.py中有BUG  

然後再回到執行topo.py的終端機，在CLI模式中輸入指定的測量指令  
> h1 iperf -s -u -i 1 -p 5566 > ./out/result2 &  

第二行輸入  
> h2 iperf -c 10.0.0.1 -u -i 1 -p 5566  

將名為h1的Host訂為Server(-s)，使用UDP協定(-u)，每經過一秒都顯示一筆數據(-i 1)，結果輸出到./out/result2，使用的port為5566(-p 5566)  
名為h2的Host訂為Client(-c)，其server的IP為10.0.0.1，使用UDP協定(-u)，每經過一秒都顯示一筆數據(-i 1)，其server使用的port為5566(-p 5566)  
然後要結束執行程式，在執行topo.py的終端機中的CLI模式中輸入  
> exit  

在執行controller.py的終端機中按Ctrl加z直接中斷程式，然後輸入  
> mn -c  

Clean up Mininet  

### Discussion

> TODO:
> * Answer the following questions

1. Describe the difference between packet-in and packet-out in detail.  

controller下達指令給OpenFlow交換器，然後OpenFlow會執行任務  
* packet-in是指將接收到的封包進行轉送到 Controller 的動作  
* packet-out是指將接收到來自 Controller 的封包轉送到指定的連接埠的動作  

> Controller 使用 Packet-In 接收來自交換器的封包之後進行分析，得到連接埠相關資料以及所連接的 host 之 MAC 位址。  
> 在學習之後，對所收到的封包進行轉送。將封包的目的位址，在已經學習的 host 資料中進行檢索，根據檢索的結果會進行下列處理。  
> * 已經存在記錄中的 host：使用 Packet-Out 功能轉送至先前所對應的連接埠  
> * 尚未存在記錄中的 host：使用 Packet-Out 功能來達到 Flooding  
   
2. What is “table-miss” in SDN?  

在收到一個封包後，會將這個封包的相關訊息進行比對，來決定將這個封包傳送出去的路徑，但是有可能會出現比對完所有條件後，仍然沒有一個條件符合這個封包，這樣就會不知道該拿這個封包怎麼辦，因此我們需要新增 Table-miss Flow Entry 到 Flow table 中，來避免當出現不符合其他所有條件的封包時不知道該怎麼辦的窘境，有點像if條件句中的else來概括所有其他條件  
   
3. Why is "`(app_manager.RyuApp)`" adding after the declaration of class in `controller.py`?  

在物件導向中class之間有繼承的關係，app_manager是import進來的library，在這個library中有名為RyuApp的class，所以在controller.py打app_manager.RyuApp，指的就是app_manager的RyuApp，寫`class SimpleController1(app_manager.RyuApp)`
就是說SimpleController1這個class是constructor and inherit from RyuApp which in app_manager  
   
4. Explain the following code in `controller.py`.
    ```python
    @set_ev_cls(ofp_event.EventOFPPacketIn, MAIN_DISPATCHER)
    ```  
    
這裡定義了當交換機收到封包後要將封包傳送出去的方法  
> * 收到的訊息若是包含 LACP data unit 的狀況下，就執行 LACP 函式庫中 LACP data unit 的處理機制  
> * 收到的訊息不包含 LACP data unit 的狀況下，則呼叫 send_event_to_observers()  
>
```python
@set_ev_cls(ofp_event.EventOFPPacketIn, MAIN_DISPATCHER)
    def packet_in_handler(self, evt):
    """PacketIn event handler. when the received packet was LACP,
    proceed it. otherwise, send a event."""
    req_pkt = packet.Packet(evt.msg.data)
    if slow.lacp in req_pkt:
        (req_lacp, ) = req_pkt.get_protocols(slow.lacp)
        (req_eth, ) = req_pkt.get_protocols(ethernet.ethernet)
        self._do_lacp(req_lacp, req_eth.src, evt.msg)
    else:
        self.send_event_to_observers(EventPacketIn(evt.msg))
```  

5. What is the meaning of “datapath” in `controller.py`?
   
6. Why need to set "`ip_proto=17`" in the flow entry?
   
7. Compare the differences between the iPerf results of `SimpleController.py` and `controller.py` in detail.
   
8. Which forwarding rule is better? Why?

---
## References

> TODO: 
> * Please add your references in the following

* **Ryu SDN**
    * [Ryubook Documentation](https://osrg.github.io/ryu-book/en/html/)
    * [Ryubook [PDF]](https://osrg.github.io/ryu-book/en/Ryubook.pdf)
    * [Ryu 4.30 Documentation](https://github.com/mininet/mininet/wiki/Introduction-to-Mininet)
    * [Ryu Controller Tutorial](http://sdnhub.org/tutorials/ryu/)
    * [OpenFlow 1.3 Switch Specification](https://www.opennetworking.org/wp-content/uploads/2014/10/openflow-spec-v1.3.0.pdf)
    * [Ryubook 說明文件](https://osrg.github.io/ryu-book/zh_tw/html/)
    * [GitHub - Ryu Controller 教學專案](https://github.com/OSE-Lab/Learning-SDN/blob/master/Controller/Ryu/README.md)
    * [Ryu SDN 指南 – Pengfei Ni](https://feisky.gitbooks.io/sdn/sdn/ryu.html)
    * [OpenFlow 通訊協定](https://osrg.github.io/ryu-book/zh_tw/html/openflow_protocol.html)
* **Python**
    * [Python 2.7.15 Standard Library](https://docs.python.org/2/library/index.html)
    * [Python Tutorial - Tutorialspoint](https://www.tutorialspoint.com/python/)
* **Others**
    * [Cheat Sheet of Markdown Syntax](https://www.markdownguide.org/cheat-sheet)
    * [Vim Tutorial – Tutorialspoint](https://www.tutorialspoint.com/vim/index.htm)
    * [鳥哥的 Linux 私房菜 – 第九章、vim 程式編輯器](http://linux.vbird.org/linux_basic/0310vi.php)

---
## Contributors

> TODO:
> * Please replace "`YOUR_NAME`" and "`YOUR_GITHUB_LINK`" into yours

* [YOUR_NAME](YOUR_GITHUB_LINK)
* [David Lu](https://github.com/yungshenglu)

---
## License

GNU GENERAL PUBLIC LICENSE Version 3
