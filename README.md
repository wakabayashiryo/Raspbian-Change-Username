# ユーザー名変更方法
SSHのIPは192.168.0.1
|変更前|→|変更後|
|:--:|:---:|:---:|
|pi||pi2|
|raspberry||raspberry2|

- ## 1. 仮ユーザー(tmp)を作成 [login : pi]   
    SSHでpiユーザーにログイン   
    > sudo ssh 192.168.0.1 -l pi   
    >> raspberry

    仮のユーザ(tmp)を作成する   
    > sudo useradd -M tmp   

    tmpユーザをsudoグループに追加(そうしないとsudoが使えない)   
    > sudo gpasswd -a tmp sudo   

    tmpユーザのパスワードを設定 (簡単なもので良い)  
    > sudo passwd tmp   
    >> hoge

    ログアウト   
    > exit       

- ## 2. ユーザー名(pi)の変更 [login : tmp]    
    SSHでtmpにログイン(rootではないのでTABの自動補完が効かない)   
    > sudo ssh 192.168.0.1 -l tmp    
    >> hoge

    usermod -lでユーザ名をpiからpi2に変更    
    > sudo usermod -l pi2 pi   

    - ### "user pi is currently used by process ***"と表示されたら
        > sudo kill ***
        
        と入力してpi権限のプロセスを停止させる

    usermod -dでホームディレクトリを/home/piから/home/pi2に変更    
    > sudo usermod -d /home/pi2 -m pi2   

    groupmod -nでpiグループをpi2グループに変更    
    > sudo groupmod -n pi2 pi    

    ログアウト    
    > exit

- ## 3. 仮ユーザー(tmp)の消去＆パスワードの変更 [login : pi2]       
    SSHでpi2ユーザーにログイン   
    > sudo ssh 192.168.0.1 -l pi2   
    >> raspberry

    仮ユーザを削除(警告を無視して削除)    
    > sudo userdel tmp    
     
    pi2ユーザのパスワードを変更    
    > sudo passwd pi2   
    >> raspberry2

- 参考サイト   
    [ユーザ名変更の個人的に「正しい」と思うやり方](calendar.google.com/calendar/r)
