# EcoStopBrakeStarter

POWER Enterprise(POWER LLC.)のEco-Stopと車両のN信号配線間に割り込ませる事で  
- ブレーキペダル操作での再始動
- アイドリングストップ有効・無効切り替え

を追加します。  
本回路およびプログラムを使用する場合、Eco-Stopはオートモードに設定してください。  
動作確認はソニカでのみ行っているため、他の車種ではプログラムの変更(START_CHECK_DELAY_TIME,START_THRESHOLD_TIME)が必要となる可能性があります。  
製作にはAVRライターが必要です。  
[Download latest release](../../releases)

# 注意事項
- 手動でD→Nと操作して再始動後すぐに再度D→Nと操作したときと同様、ブレーキペダルでの再始動後の数秒間はEco-Stopがスターターを回そうとするため、NもしくはNを経由するバックに入れたい場合は再始動後少し待ってからシフト操作してください。
    - Eco-Stop取扱説明書に記載されているエンスト時の再始動動作です。
- 有効・無効切り替えを何度も操作していると稀にアイドリングストップされなくなり、シフトレバーをNに入れた際にセルが回される状態になることがあります。
    - Eco-Stop取扱説明書に記載の再始動動作に入っているものと思われます。
    - 手動でキーを操作し、エンジンを一度停止してから再始動することで復帰可能です。
        - アイドリングストップ有効にして暫く待つことでも復帰可能だろうと思われますが、再現できていないため未確認です。
    - アイドリングストップされなくなった状態で有効・無効切り替えスイッチを切り替えるとEco-Stopからリレー音が聞こえる場合はこの状態になっていると判断できます。

# 再始動操作動画
ブレーキペダル操作での再始動  
[![ブレーキペダル操作での再始動](https://img.youtube.com/vi/8QJhcB1Uzlw/0.jpg)](https://www.youtube.com/watch?v=8QJhcB1Uzlw)

アイドリングストップ有効・無効切り替えスイッチ操作での再始動  
[![切替スイッチ操作での再始動](https://img.youtube.com/vi/qSqv4JRvUJY/0.jpg)](https://www.youtube.com/watch?v=qSqv4JRvUJY)

シフト操作での再始動  
[![シフト操作での再始動](https://img.youtube.com/vi/vhFaXYEGKy0/0.jpg)](https://www.youtube.com/watch?v=vhFaXYEGKy0)

# 回路図
スターターカットリレーがインヒビタースイッチの上流に取り付けられている車種用(i-Smartの車種別配線図でリレーBと記載)です。
- リレーはコイル消費電流が100mA以下のものを使用してください。
- ブレーキには数A(ソニカで約5A)流れるため、使用可能電力に余裕を持ったリレーを使用してください。
- キーをIGNからACCに切り替えた後2～4秒程度でリレーがOFFとなるように可変抵抗を調整してください。
- EcoStop用N信号とPB4の間の抵抗は配線が車体に接触してしまった際にATTiny13Aの出力端子破損を防ぐためのものなので、接続配線のEco-Stop側ではなくATTiny13A側にとりつけてください。  
![EcoStopBrakeStarter.png](/CircuitDiagrams/EcoStopBrakeStarter.png "EcoStopBrakeStarter")

以下の切替回路を追加することでスターターカットリレーがインヒビタースイッチの下流に取り付けられている車種(i-Smart車種別配線図でリレーAと記載)と共用に出来ますが、動作未確認です。  
常時電源とつながるヒューズ部分は上記回路のものと共用できます。
![StarterCutRelay.png](/CircuitDiagrams/StarterCutRelay.png "StarterCutRelay")

# 動作確認
接続後は公道を走行する前に一度安全な場所で動作確認を行ってください。
- 停車状態での確認
    - アイドリングストップスイッチONでの確認
        1. キーをIGNにし、エンジンを始動せずにブレーキペダルを踏んで離すとすぐにブレーキランプが消えること。
        2. エンジンを始動しブレーキペダルを踏んで離すとすぐにブレーキランプが消えること。
        3. キーをACCにしてエンジンを停止し、2秒以内にIGNに戻し、ブレーキペダルを踏んで離すとブレーキランプが約4秒間点灯して消えること。
        4.再度ブレーキペダルを踏んで離すとすぐにブレーキランプが消えること。
    - アイドリングストップスイッチOFFでの確認
        1. エンジンを始動し、キーをACCにしてエンジンを停止し、2秒以内にONに戻し、ブレーキペダルを踏んで離すとすぐにブレーキランプが消えること。
- 走行しての確認
    - アイドリングストップスイッチONでの確認
        1. 30km/h以上で走行し、シフトレバーをDに入れたままブレーキを踏んで停車し、アイドリングストップされた後、シフトレバーをNに入れるとエンジンが再始動されること。
        2. 30km/h以上で走行し、シフトレバーをDに入れたままブレーキを踏んで停車し、アイドリングストップされた後、ブレーキを離すとエンジンが再始動されること。
        3. 30km/h以上で走行し、シフトレバーをDに入れたままブレーキを踏んで停車し、アイドリングストップされる前にシフトレバーをNに入れるとアイドリングストップされないこと。
        4. 30km/h以上で走行し、シフトレバーをDに入れたままブレーキを踏んで停車し、アイドリングストップされる前にブレーキを離し、アイドリングストップされた後、ブレーキを踏んで離すとエンジンが再始動されること。
        5. 30km/h以上で走行し、シフトレバーをDに入れたままブレーキを踏んで停車し、アイドリングストップされる前にアイドリングストップスイッチをOFFにするとアイドリングストップされないこと。
        6. 30km/h以上で走行し、シフトレバーをDに入れたままブレーキを踏んで停車し、アイドリングストップされた後にアイドリングストップスイッチをOFFにするとエンジンが再始動されること。
    - アイドリングストップスイッチOFFでの確認
        1. 30km/h以上で走行し、シフトレバーをDに入れたままブレーキを踏んで停車し、アイドリングストップされないこと。


# 免責事項
本回路およびプログラムを使用して発生した損害やその修理費用等に関して、一切の責任を負いません

`Apache License 2.0`
