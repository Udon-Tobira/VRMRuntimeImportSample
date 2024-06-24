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
[平行移動する様子](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/ac3f05a7-448b-4338-ac4e-b4e7f30241e6/animation.mov)

### アニメーションのリターゲティング
- 特定のスケルタルメッシュ用に作られたアニメーションを、別のスケルタルメッシュに適用することを「リターゲティング」という。
- 今回は、サードパーソンのマネキン用に作られたアニメーションを流用するので、リターゲティングが必要。
1. サードパーソンテンプレートにある RTG_Mannequin ファイル(IK Retargeter 型)を開く。
2. Target IKRig Asset に IK_(モデル名)_Mannequin をセットする。
![Target IKRig Assetのセット](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/ca0a57da-8382-4103-876f-3b48e99c47dd/Untitled.png)
3. するとこのように左右に比較表示されるのだが、恐らく左と右でポーズが違うと思う。左をAポーズ、右をTポーズという。このポーズを揃えないと適切にアニメーションが設定されない。
    ![左右でポーズが異なる](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/307864ea-5fda-41c2-99a6-88e982d77cb6/Untitled.png)
    <details>
    <summary>ポーズを揃えるには</summary>

    1. 「Running Retarget」をクリックして、「Editing Retarget Pose」モードにする。
    ![Running Retargetボタン](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/fbef2069-b543-4753-a5fb-f92ad54979f8/Untitled.png)
    2. [Source] の設定で、Current Retarget Pose を「T Pose」に変更する。
    ![Current Retarget Poseの設定](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/6ef563a1-2565-4ec9-bd3d-5833badab76c/Untitled.png)
    そしたら揃う
    ![ポーズが揃った後の状態](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/46c31c66-babf-4213-b00c-44b5ea2ca9f6/Untitled.png)
    </details>
4. 右下の Asset Browser から好きなアニメーション（例：MF_Walk_Fwd）をダブルクリックするとプレビューできる。うまく動けばこれはOK。うまく動かなければ、いろいろ調べて直してね＾＾；
![アニメーションアセット](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/f0dcff5b-756d-4b5d-9f39-da2a2fc8dfaf/Untitled.png)
[走るアニメーション](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/8457d7ba-7907-4979-b574-23f66b407df8/2024-06-24_16-05-42.mkv)
6. 流用したいアニメーションブループリント（今回の場合、ABP_Manny）の上で右クリックし、Retarget Animations を押す。
![Retarget Animationsボタン](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/d9b9b3c1-44fe-405f-8964-1c64aa37e622/Untitled.png)
7. Target Skeletal Mesh にインポートしたモデルのスケルタルメッシュを設定。  
Auto Generate Retargeter のチェックを外し、
Retarget Asset に RTG_Mannequin をセットする。
![Retarget Animationsの設定](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/b487613f-7cf3-4b8a-a5d7-737de3c8d9d8/Untitled.png)
8. エクスポートしたいアニメーション（今回は、ABP_Manny を選択）して、Export Animations
![Export UIの様子](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/8cfdb34e-60fb-4c48-ae2b-8ecabbf7e0ef/Untitled.png)
9. エクスポート先を設定して（たくさんのファイルが追加されるので新しいフォルダがおススメ）、必要ならばファイルの名前を変更するオプションを入力し、エクスポート、エクスポート。
![エクスポート場所の指定](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/ed5dedc8-5834-4815-8cd2-162785bdf95c/Untitled.png)
![エクスポートオプション](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/24cef5f7-dee5-439a-94df-f89b75e8174f/Untitled.png)
10. BP_ThirdPersonCharacter を開き、Details ウィンドウの、Animation > Anim Class にエクスポートしたアニメーションブループリントを設定。実行するとちゃんと動く。
[実際に動いている様子](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/62f097a8-f349-4d25-afce-f60c51964b72/2024-06-24_16-36-17.mkv)

### ジャンプ後の着地時に起こる異常の修正
[着地時の挙動がおかしい](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/2ca8c976-a558-42d5-9b05-249b9764953c/2024-06-24_16-47-18.mkv)
- 上記の設定で実行してジャンプして着地すると、着地時に異常な表示が起こり、また、ゲーム終了時に「Trying to play a non-additive animation 'MM_Land_Aneki' into a pose that is expected to be additive in anim instance 'ABP_Aneki_C /Game/UEDPIE_0_Lobby.Lobby:PersistentLevel.BP_ThirdPersonCharacter_C_0.CharacterMesh0.ABP_Aneki_C_0’」のような警告が出る。
- これは、IK Retargeter のアニメーションエクスポート時に、アニメーションの Additive Settings が反映されないというバグらしい。ということで、この項目のみ手動で設定すればOK
1. 対象アニメーションの元ファイル(今回は、着地時のアニメーション MM_Land)を開く
2. エクスポート先アニメーションのファイル(今回は、MM_Land_Aneki)を開き、Additive Settings を 1. で開いたファイルと同じ状態にする。すると、治る。
![設定している様子](https://prod-files-secure.s3.us-west-2.amazonaws.com/8e31839c-0c4f-4443-bd1e-3ae473d4e08f/efb9a520-d8c6-47da-a8e8-0f16f062a619/Untitled.png)
