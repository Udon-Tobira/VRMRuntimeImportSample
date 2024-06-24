# VRMImportSample

VRMをエディター時と、ゲーム時の両方でインポートできるようにするサンプルプロジェクト。
[VRM4Uプラグイン](https://ruyo.github.io/VRM4U/01_quick-start/)を使用しています。


## エディター使用時にインポートして、操作可能なサードパーソンキャラクターとして設定する方法
以下の説明や、本プロジェクトにインポートされているキャラクターのVRMファイルは、[VRM Model "Aneki"](https://booth.pm/ja/items/3257189) を使わせていただいております。

### メッシュのインポート
1. プロジェクトをサードパーソンテンプレートで作成するか、以下必要なものを随時移行してくる。
2. VRM のインポートをできるようにするプラグイン(VRM4U)のダウンロードをする。  
[VRM4U-セットアップ](https://ruyo.github.io/VRM4U/01_quick-start/)
3. プロジェクトを開き、コンテンツフォルダにVRMファイルをドラッグ&ドロップ。(なお、インポートすると大量のファイルが追加されるので、事前にモデル専用のフォルダを用意しておくと良い、またこの後アニメーションもアセットが増えるので、アニメーション用のフォルダもあると良い)  
例：Content/(モデル名)/Mesh と Content/(モデル名)/Animations を作っておく
4. インポートの設定画面では、Material Type を MToon Lit に設定してインポート。  
![Untitled](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/f2537002-c054-4177-85b4-5574249743e9)
5. BP_ThirdPersonCharacter を開き、Details ウィンドウの、Mesh > Skeletal Mesh Asset にインポートしたスケルタルメッシュを設定。
6. これで起動すると、そのキャラで動かせるが、アニメーションを適切に設定していないので、T字のまま平行移動することになる。以降でアニメーションの設定をしていく。  
[平行移動している様子の動画](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/965cc6d7-c431-4830-b94f-eb31f55dcde8)

### アニメーションのリターゲティング
- 特定のスケルタルメッシュ用に作られたアニメーションを、別のスケルタルメッシュに適用することを「リターゲティング」という。
- 今回は、サードパーソンのマネキン用に作られたアニメーションを流用するので、リターゲティングが必要。
1. サードパーソンテンプレートにある RTG_Mannequin ファイル(IK Retargeter 型)を開く。
2. Target IKRig Asset に IK_(モデル名)_Mannequin をセットする。  
![342349324-4b8ae001-4ac2-4190-bbb3-83d9fe676e3c](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/2d3b4785-00f8-4c5a-b7b9-f74736acdd2e)
3. するとこのように左右に比較表示されるのだが、恐らく左と右でポーズが違うと思う。左をAポーズ、右をTポーズという。このポーズを揃えないと適切にアニメーションが設定されない。  
    ![342349475-8767c46f-183b-4d3a-b380-3fcc2d6e4e68](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/e4a65cbf-8a6e-40b8-b269-a08f67ebe682)

    <details>
    <summary>ポーズを揃えるには</summary>

    1. 「Running Retarget」をクリックして、「Editing Retarget Pose」モードにする。  
    ![342349751-d45987f2-1715-45ab-bfb2-55268d39a9db](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/96402157-16c4-4b16-94e4-46e8824dab0d)
    2. 「Source」の設定で、Current Retarget Pose を「T Pose」に変更する。  
    ![342349769-ebe79543-fc01-4fc1-b542-b49f6447f155](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/4ed4fd32-710f-4ea4-b10e-174a04b78938)
    そしたら揃う  
    ![342349803-5245e991-68ea-4753-85e1-a87523d44280](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/236aa881-5a3c-470f-97fe-9d5c6b63db9d)
    </details>
4. 右下の Asset Browser から好きなアニメーション（例：MF_Run_Fwd）をダブルクリックするとプレビューできる。うまく動けばこれはOK。うまく動かなければ、いろいろ調べて直してね＾＾；  
![342350048-5f67f729-99b3-4e2d-9676-c776cf3f4e28](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/c8a69f1d-59da-40c7-bf00-5c1cddc5d842)  
[結果の動画](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/6b76c94b-3910-46c1-9771-2531a7185dec)
5. 流用したいアニメーションブループリント（今回の場合、ABP_Manny）の上で右クリックし、Retarget Animations を押す。  
![342350802-b7c9f1bf-ad6e-49b9-97e0-12969d9be96e](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/803c0cbd-5c98-4217-950e-6e0dc0205f7f)
6. Target Skeletal Mesh にインポートしたモデルのスケルタルメッシュを設定。    
Auto Generate Retargeter のチェックを外し、  
Retarget Asset に RTG_Mannequin をセットする。  
![342350939-d756db54-7892-411e-93df-e3780ae3ae14](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/fd70f9d4-22f9-4a6e-bbff-a137f4db9415)
7. エクスポートしたいアニメーション（今回は、ABP_Manny を選択）して、Export Animations  
![342351023-ed796408-a3c9-4f3b-a6cf-160b7e83f3fc](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/0a5619b5-7d81-4b25-99ce-37bcf6e1d9ee)
8. エクスポート先を設定して（たくさんのファイルが追加されるので新しいフォルダがおススメ）、必要ならばファイルの名前を変更するオプションを入力し、エクスポート、エクスポート。  
![342351157-bd6689b3-ce21-4895-aa61-66b78e90d276](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/1dbb9a00-c741-4bf9-ba0e-546024912e2f)  
![342351198-93d425b1-6ce5-4c5a-ac35-8fdce58f73a5](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/d38aa06e-aa2c-43f9-921a-36d19b8143af)
9. BP_ThirdPersonCharacter を開き、Details ウィンドウの、Animation > Anim Class にエクスポートしたアニメーションブループリントを設定。実行するとちゃんと動く。
[結果の動画](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/d4c29060-6579-4978-bd3f-481bf9dc38b4)

### ジャンプ後の着地時に起こる異常の修正

- 上記の設定で実行してジャンプして着地すると、着地時に異常な表示が起こり、また、ゲーム終了時に「Trying to play a non-additive animation 'MM_Land_Aneki' into a pose that is expected to be additive in anim instance 'ABP_Aneki_C /Game/UEDPIE_0_Lobby.Lobby:PersistentLevel.BP_ThirdPersonCharacter_C_0.CharacterMesh0.ABP_Aneki_C_0’」のような警告が出る。
- これは、IK Retargeter のアニメーションエクスポート時に、アニメーションの Additive Settings が反映されないというバグらしい。ということで、この項目のみ手動で設定すればOK
[ジャンプと着地の動画](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/c148c8de-16c4-44eb-9828-bda1d6612d65)

1. 対象アニメーションの元ファイル(今回は、着地時のアニメーション MM_Land)を開く
2. エクスポート先アニメーションのファイル(今回は、MM_Land_Aneki)を開き、Additive Settings を 1. で開いたファイルと同じ状態にする。すると、治る。  
![342351382-6bdbd2a4-7ecb-4ba8-bb4d-79c2f719611b](https://github.com/Udon-Tobira/VRMImportSample/assets/146440502/84c1f1c1-5e26-439a-9f54-8b922367be99)

