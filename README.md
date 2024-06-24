# VRMImportSample

VRMをエディター時と、ゲーム時の両方でインポートできるようにするサンプルプロジェクト。
[VRM4Uプラグイン](https://ruyo.github.io/VRM4U/01_quick-start/)を使用しています。

## エディター使用時にインポートして、操作可能なサードパーソンキャラクターとして設定する方法

### メッシュのインポート
1. プロジェクトをサードパーソンテンプレートで作成するか、以下必要なものを随時移行してくる。
2. VRM のインポートをできるようにするプラグイン(VRM4U)のダウンロードをする。  
[VRM4U-セットアップ](https://ruyo.github.io/VRM4U/01_quick-start/)
3. プロジェクトを開き、コンテンツフォルダにVRMファイルをドラッグ&ドロップ。(なお、インポートすると大量のファイルが追加されるので、事前にモデル専用のフォルダを用意しておくと良い、またこの後アニメーションもアセットが増えるので、アニメーション用のフォルダもあると良い)  
例：Content/(モデル名)/Mesh と Content/(モデル名)/Animations を作っておく
4. インポートの設定画面では、Material Type を MToon Lit に設定してインポート。
![VRMのインポート](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/284b2989-13b3-4e07-8a57-8b7514fef013/Untitled.png)
5. BP_ThirdPersonCharacter を開き、Details ウィンドウの、Mesh > Skeletal Mesh Asset にインポートしたスケルタルメッシュを設定。
6. これで起動すると、そのキャラで動かせるが、アニメーションを適切に設定していないので、T字のまま平行移動することになる。以降でアニメーションの設定をしていく。
https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/965cc6d7-c431-4830-b94f-eb31f55dcde8

### アニメーションのリターゲティング
- 特定のスケルタルメッシュ用に作られたアニメーションを、別のスケルタルメッシュに適用することを「リターゲティング」という。
- 今回は、サードパーソンのマネキン用に作られたアニメーションを流用するので、リターゲティングが必要。
1. サードパーソンテンプレートにある RTG_Mannequin ファイル(IK Retargeter 型)を開く。
2. Target IKRig Asset に IK_(モデル名)_Mannequin をセットする。
![Untitled](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/4b8ae001-4ac2-4190-bbb3-83d9fe676e3c)
3. するとこのように左右に比較表示されるのだが、恐らく左と右でポーズが違うと思う。左をAポーズ、右をTポーズという。このポーズを揃えないと適切にアニメーションが設定されない。
    ![Untitled (1)](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/8767c46f-183b-4d3a-b380-3fcc2d6e4e68)

    <details>
    <summary>ポーズを揃えるには</summary>

    1. 「Running Retarget」をクリックして、「Editing Retarget Pose」モードにする。
    ![Untitled (2)](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/d45987f2-1715-45ab-bfb2-55268d39a9db)
    2. [Source] の設定で、Current Retarget Pose を「T Pose」に変更する。
    ![Untitled (3)](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/ebe79543-fc01-4fc1-b542-b49f6447f155)
    そしたら揃う
    ![Untitled (4)](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/5245e991-68ea-4753-85e1-a87523d44280)
    </details>
5. 右下の Asset Browser から好きなアニメーション（例：MF_Walk_Fwd）をダブルクリックするとプレビューできる。うまく動けばこれはOK。うまく動かなければ、いろいろ調べて直してね＾＾；
![Untitled (5)](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/5f67f729-99b3-4e2d-9676-c776cf3f4e28)

6. 流用したいアニメーションブループリント（今回の場合、ABP_Manny）の上で右クリックし、Retarget Animations を押す。
![Untitled (6)](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/b7c9f1bf-ad6e-49b9-97e0-12969d9be96e)
7. Target Skeletal Mesh にインポートしたモデルのスケルタルメッシュを設定。  
Auto Generate Retargeter のチェックを外し、
Retarget Asset に RTG_Mannequin をセットする。
![Untitled (7)](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/d756db54-7892-411e-93df-e3780ae3ae14)
8. エクスポートしたいアニメーション（今回は、ABP_Manny を選択）して、Export Animations
![Untitled (8)](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/ed796408-a3c9-4f3b-a6cf-160b7e83f3fc)
9. エクスポート先を設定して（たくさんのファイルが追加されるので新しいフォルダがおススメ）、必要ならばファイルの名前を変更するオプションを入力し、エクスポート、エクスポート。
![Untitled (9)](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/bd6689b3-ce21-4895-aa61-66b78e90d276)
![Untitled (10)](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/93d425b1-6ce5-4c5a-ac35-8fdce58f73a5)
10. BP_ThirdPersonCharacter を開き、Details ウィンドウの、Animation > Anim Class にエクスポートしたアニメーションブループリントを設定。実行するとちゃんと動く。


### ジャンプ後の着地時に起こる異常の修正

- 上記の設定で実行してジャンプして着地すると、着地時に異常な表示が起こり、また、ゲーム終了時に「Trying to play a non-additive animation 'MM_Land_Aneki' into a pose that is expected to be additive in anim instance 'ABP_Aneki_C /Game/UEDPIE_0_Lobby.Lobby:PersistentLevel.BP_ThirdPersonCharacter_C_0.CharacterMesh0.ABP_Aneki_C_0’」のような警告が出る。
- これは、IK Retargeter のアニメーションエクスポート時に、アニメーションの Additive Settings が反映されないというバグらしい。ということで、この項目のみ手動で設定すればOK
1. 対象アニメーションの元ファイル(今回は、着地時のアニメーション MM_Land)を開く
2. エクスポート先アニメーションのファイル(今回は、MM_Land_Aneki)を開き、Additive Settings を 1. で開いたファイルと同じ状態にする。すると、治る。
![Untitled (11)](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/6bdbd2a4-7ecb-4ba8-bb4d-79c2f719611b)
