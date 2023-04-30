# bof5
## Ý tưởng.
- Ngày hôm nay ta lại đến với 1 kỹ thuật mới là ret2shellcode, nhìn vào bài hôm nay thì cụ thể sẽ như sau.
![image](https://user-images.githubusercontent.com/130078745/235352608-5dbe52e1-2a2c-4b7c-a4c7-579a9a760719.png)
![image](https://user-images.githubusercontent.com/130078745/235352616-5d62efe4-d9ca-4dd7-a9d8-4fd5f80510ec.png)
- Với 1 hàm main và 1 hàm run , vào sâu trong hàm run thì ta có thể thấy ở lênh **read** thứ 2 người ta nhập vào nhiều hơn số lượng byte quy định thế và đây là 1 lỗi **BOF** và ý tưởng của bài này là t sẽ phải tự tạo 1 đoạn code assembly để có thể in ra shell.
## Tự hành.
- Trước tiên ta cung vào gdb để debug xem lỗi nó diễn ra như thê nào.
- Cùng tạo 544 byte dữ liệu để nhập vào **V2** thử xem sẽ diễn ra như nào , và ta cùng chạy thử đề bài.
![image](https://user-images.githubusercontent.com/130078745/235352941-ad873f9c-d851-4fad-b3ec-484642a1cc45.png)
-Sau khi nhập dữ liệu và nhảy đến lệnh **ret** 8 byte dữ liệu nhập vào **v2** lại nhảy vào địa chỉ như hình, tức là ta đã overwrite cái save rip và có thể điều kiển đc chương trình.
![image](https://user-images.githubusercontent.com/130078745/235353039-854b886e-2428-41b9-8cb3-036df308a138.png)
- Xài thử câu lệnh **checksec** , ta có thể thấy bit NX tắt đồng nghĩa với thanh ghi **stack** thực thi đc.
![image](https://user-images.githubusercontent.com/130078745/235353099-d83190b0-f1e2-4a0c-929b-a84c8d987511.png)
![image](https://user-images.githubusercontent.com/130078745/235353150-d4a0d048-7e4f-422b-a129-2c5610804f9f.png)
- Ta sẽ đưa những đoạn mã assembly đưa lên stack để nó thực hiện chương trình.
- Ta sẽ tiến hành viết tools để có thể tạo nên shell code và sử dụng pwntool để từ các mã assembly ta có thể lấy đc byte shell code và hoàn thành bài hôm lay.
![image](https://user-images.githubusercontent.com/130078745/235353443-b7d3c389-a561-43e8-92ed-c9c19c8b8fa3.png)
![image](https://user-images.githubusercontent.com/130078745/235353646-36fbed1f-b298-43cc-b1bc-ba017aef895a.png)
- Ta có thể làm như sau : bước đầu ta cần nhập đoạn mã assembly tạo shell code vào chỗ nhập đầu tiên và bước tiếp theo có 2 cách . 1 là ta thay đổi **save rip** để biến **V2** trỏ vào địa chỉ của lần nhập đầu tiên  , cách 2 là t sử dụng gadget để biến **v2** có thể trỏ đến thanh ghi **$rax** của lần nhập đầu.
- Trước tiên ta cùng tìm thử có gadget nào thực hiện đc việc đó ko.
![image](https://user-images.githubusercontent.com/130078745/235353836-dff69cb7-56f5-4b03-9147-755a86b247c4.png)
- Tìm thử thì ta tìm đc 2 gadget sau ( chức năng là tương đương nhau nên xài cái nào cũng đc ) , vì có gadget có thể tró trực tiếp vào thanh ghi rax nên ta sẽ tận dụng cách này để hoàn thành bài nhanh chóng hơn.
- Ta lưu trữ lại địa chỉ của **call rax** vào tool, lần nhập đầu tiên ta sẽ nhạp vào đấy shell code, và ở lần nhập tiếp theo ta sẽ cùng nhập vào 536 byte và 8 byte dữ liệu sau khi đã overwrite save rip bằng địa chỉ chứa dữ liệu của shell code.
![image](https://user-images.githubusercontent.com/130078745/235354220-b78d4a7f-de1f-4168-b15c-a97b3f17db64.png)
- Cùng chạy thử tools đã viết nhóe :>>> , sau khi chạy thử thì ta đã lấy đc shell.
![image](https://user-images.githubusercontent.com/130078745/235356862-22ad8d46-a6c7-4391-aa97-0ba69eee729a.png)

