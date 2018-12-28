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
![alt text](https://github.com/nctucn/lab3-jillkuo/blob/master/src/lab3_png/run_topo.png)  

第二個終端機再輸入  
> ryu-manager SimpleController.py --observe-links  

意思是使用Ryu Controller(ryu-manager <PYTHON FILE NAME>.py --observe-links)，controller的規則為SimpleController.py，若要以另一種規則來進行比較，則將SimpleController.py改寫為controller.py，以另一個py檔來定義新規則  

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
名為h1的Host訂為Client(-c)，其server的IP為10.0.0.1，使用UDP協定(-u)，每經過一秒都顯示一筆數據(-i 1)，其server使用的port為5566(-p 5566)  

result1  
![alt text](https://github.com/nctucn/lab3-jillkuo/blob/master/src/lab3_png/run_result1.png)  

result2  
![alt text](https://github.com/nctucn/lab3-jillkuo/blob/master/src/lab3_png/run_result2.png)  

---
## Description

### Tasks

> TODO:
> * Describe how you finish this work in detail

1. Environment Setup

2. Example of Ryu SDN

3. Mininet Topology

4. Ryu Controller

5. Measurement

### Discussion

> TODO:
> * Answer the following questions

1. Describe the difference between packet-in and packet-out in detail.
   
2. What is “table-miss” in SDN?
   
3. Why is "`(app_manager.RyuApp)`" adding after the declaration of class in `controller.py`?
   
4. Explain the following code in `controller.py`.
    ```python
    @set_ev_cls(ofp_event.EventOFPPacketIn, MAIN_DISPATCHER)
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
