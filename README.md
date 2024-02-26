# NIST Special Publication 800-63B Digital Identity Guidelines (翻訳版)
https://doi.org/10.6028/NIST.SP.800-63b    

1. [目的](#1-目的)
2. [はじめに](#2-はじめに)
3. [定義及び略語](#3-定義及び略語)
4. [認証器の保証レベル](#4-認証器の保証レベル)
5. [認証器及び検証者要件](#5-認証器及び検証者要件)
6. [認証器ライフサイクルの要件](#6-認証器ライフサイクルの要件)
7. [セッション管理](#7-セッション管理)
8. [脅威とセキュリティに関する考慮事項](#8-脅威とセキュリティに関する考慮事項)
9. [プライバシに関する考慮事項](#9-プライバシーの考慮事項)
10. [ユーザビリティに関する考慮事項](#10-ユーザビリティに関する考慮事項)
11. [参照](#11-参照)
12. [付録A &mdash; 記憶シークレットの強度](#付録-a記憶シークレットの強度)
13. [付録B &mdash; 定義](#付録-b定義と略語)

## 1 目的
_本セクションは参考情報である._

本書及び付随文書である[Special Publication (SP) 800-63](sp800-63-3.html)，[SP 800-63A](sp800-63a.html)および[SP 800-63C](sp800-63c.html)は，政府機関に対してDigital Authenticationの実装のための技術的なガイドラインを提供する．

## 2 はじめに
_本セクションは参考情報である._

Digital Identityはオンライン取引に関わるSubjectの一意な表現である．Digital Identityはデジタルサービスの文脈では常に一意だが，必ずしも実在するSujectまで追跡できる必要はない．言い換えれば，デジタルサービスにアクセスするということが，根本的なSubjectの実在の表現が知られていることを意味していなくてもよい．Identity ProofingはSubjectが実際には彼らが何者であるかを確認する．Digital Authenticationは，Digital Identityを主張するために利用いる一つ以上のAuthenticatorの妥当性を決定するプロセスである．Authentcationはデジタルサービスに対してアクセスしようとしているSubjectが，Authenticateするための技術を制御しているということを確認する行為である．再訪問するようなサービスでは，正常にAuthenticateすることが今日サービスにアクセスしているSubjectが以前サービスにアクセスした人と同一であるということを示す合理的なリスクベースの保証をもたらす．Digital Identityは多くの場合オープンなネットワークを介した個人のProofingを伴い，常にオープンなネットワークを介した個人のAuthenticationが必要となるため技術的な技術的な課題がある．この課題には，SubjectのDigital Identityを不正にかたることにつながるなりすましや他の攻撃の可能性が複数存在している．

Subscriberの継続的なAuthenticationは，Subscriberを彼らのオンラインでの活動と結びつけるプロセスの中心である．Subscriber Authenticationは，Claimantが指定されたSubscriberと関連付けられている一つ以上の*Authenticator*(以前のバージョンのSP 800-63では*token*と呼ばれていたもの)を制御していることを検証することで実施される．Authenticationが成功した結果は，Relying Party(RP)に対する仮名または仮名でない識別子のAssertionであり，オプションで他のIdentity情報も対象となる．

本書は，様々な*Authenticator Assurance Level*(AAL)で利用されるAuthenticatorの選択を含むAuthenticationプロセスの分類における推奨を提供する．さらに本書は，紛失や盗難に際しての取り消しを含む，Authenticatorのライフサイクルにおける推奨を提供する．

この技術的ガイドラインは，ネットワーク越しのシステムに対するSubjectのDigital Authenticationに適用される．ガイドラインは(ビルなどへの)物理アクセスのための人物のAuthenticationについては考慮していないが，デジタルアクセスのために利用されるものと同じクレデンシャルが，物理アクセスのAuthenticationの目的でも利用されてよい．さらにこの技術的ガイドラインは，Authenticationプロトコルに参加する連邦政府機関のシステム及びサービスの提供者が，Subscriberに対してAuthenticateされることを要求する．

Authenticationトランザクションの強度はAALとして知られている分類によって特徴付けられる．より強力な認証(より高いAAL)では，悪意のある当事者がAuthenticationプロセスを成功裏に覆すために一層の機能及びリソースが必要となる．より高いAALでのAuthenticationは攻撃リスクを効果的に減少させることができる．個々のAALに求められる技術要件の概要を以下に記載する; 特別な標準要求については，本書の [Section 4](#sec4) 及び [5](#sec5) を参照．

**Authenticator Assurance Level 1** - AAL 1はSubscriberのアカウントに対して結び付けられているAuthenticatorをClaimantが制御しているというある程度の保証をもたらす．AAL 1では幅広く利用可能なAuthentication技術を利用した，単一要素または多要素のAuthenticationを必要とする．Authenticationが成功するには，そのClaimantが，セキュアなAuthenticationプロトコルを介して，自身がそのAuthenticatorを所有・制御していることを証明する必要がある．

**Authenticator Assurance Level 2** - AAL 2は，Subscriberのアカウントに対して結び付けられているAuthenticatorをClaimantが制御しているという高い確実性をもたらす．セキュアなAuthenticationプロトコルを介して，異なる2つのAuthentication要素を所有・制御していることを証明する必要がある．AAL 2及びそれ以上では，Approved Cryptography技術が必要とされる．

**Authenticator Assurance Level 3** - AAL 3は，Subscriberのアカウントに対して結び付けられているAuthenticatorをClaimantが制御しているという非常に高い確実性をもたらす．AAL 3におけるAuthenticationは，暗号プロトコルを介した鍵を所有していることの証明に基づいている．AAL 3のAuthenticationは，ハードウェアベースのAuthenticatorまたはVerifierに対してなりすまし耐性を提供するAuthenticatorを要求す．この際，同じデバイスが両方の要件を満たしていても良い．AAL 3でAuthenticateするために，ClaimantはセキュアなAuthenticationプロトコルを介して2つの異なるAuthentication要素を所有・制御していることを証明する必要がある．Approved Cryptography技術が必要とされる．

次の表は本文書の各セクションが標準であるか参考情報であるかを示している:

|セクション名|標準/参考情報
|----|:--:|
|1. 目的|参考情報|
|2. はじめに|参考情報|
|3. 定義及び略語|参考情報|
|4. Authenticator Assurance Levels|標準|
|5. Authenticator及びVerifierの要件|標準|
|6. Authenticatorライフサイクルの要件|標準|
|7. セッション管理|標準|
|8. 脅威とセキュリティに関する考慮事項|参考情報|
|9. プライバシに関する考慮事項|参考情報|
|10. ユーザビリティに関する考慮事項|参考情報|
|11. 参照|参考情報|
|付録A &mdash; 記憶シークレットの強度|参考情報|

## 3 定義及び略語
定義及び略語の全体一式については[付録B.定義と略語](#付録-b定義と略語)を参照．

## 4 認証器の保証レベル
_本セクションは標準及び参考情報の題材両方を含む._

指定されたAALの要件を満たすために，Claimantは少なくとも自身がSubscriberとして認識される強度のレベルでAuthenticateされるものとする(SHALL)．Authenticationプロセスの結果は識別子であり，SubscriberがRPに対してAuthenticateするたびに使われるものとする(SHALL)．それは仮名でもよい(MAY)，Subscriberの識別子は異なる目的で再利用すべきではない(SHOULD NOT)が，CSPによって過去に登録済みのSubjectが再登録される場合には再利用すべきである(SHOULD)．Subscriberをを一意なSubjectであると識別する他の属性もまた提供されてもよい(MAY)．

各AALにおけるAuthenticator及びVerifierに対する標準要件の詳細についてはセクション5に示されている．

最も適切なAALの選択方法の詳細については [SP 800-63](sp800-63-3.html) セクション6.2参照．

FIPS 140 要件は，[FIPS 140-2](#FIPS140-2)またはより新しい版により満たされる．

IAL1では，属性はDigital Identityサービスによって収集され，利用可能となる．任意のPIIまたは他の個人情報は - self-assertedまたは確認されたもののどちらでも - 多要素Authenticationを必要とする．従って，連邦政府機関はself-assertedであるPIIや他の個人情報がオンラインで利用可能である場合，最低でもAAL2を選択するものとする(SHALL)．

### 4.1 Authenticator Assurance Level 1
*本セクションは標準である．*

AAL 1はSubscriberのアカウントに対して結び付けられているAuthenticatorをClaimantが制御しているという，ある程度の保証を提供する．AAL 1では幅広く利用可能なAuthentication技術を利用した，単一要素または多要素のAuthenticationを必要とする．Authenticationが成功するには，そのClaimantが，セキュアなAuthenticationプロトコルを介して，自身がそのAuthenticatorを所有・制御していることを証明する必要がある．

#### 4.1.1 許可されているAuthenticatorタイプ
AAL1のAuthenticationでは，[Section 5](#sec5) で定義されている次のAuthenticatorタイプの何れかを利用するものとする(SHALL):

* 記憶シークレット ([Section 5.1.1](#511-記憶シークレット))
* ルックアップシークレット ([Section 5.1.2](#512-ルックアップシークレット))
* アウトオブバンドデバイス  ([Section 5.1.3](#513-アウトオブバンドデバイス))
* 単一要素ワンタイムパスワード (OTP) デバイス ([Section 5.1.4](#514-単一要素otpverifier))
* 多要素 OTP デバイス ([Section 5.1.5](#515-multi-factor-otp-devices))
* 単一要素暗号ソフトウェア ([Section 5.1.6](#sfcs))
* 単一要素暗号デバイス ([Section 5.1.7](#sfcd))
* 多要素暗号ソフトウェア ([Section 5.1.8](#mfcs))
* 多要素暗号デバイス ([Section 5.1.9](#mfcd))

#### 4.1.2 Authenticator及びVerifierの要件
AAL 1で用いられる暗号Authenticatorは，Approved Cryptographyを利用するものとする(SHALL)．オペレーティング・システム環境で動作するソフトウェアベースのAuthenticatorは，該当する場合(例，マルウェアによって)それ自身が動作している利用者のエンドポイントのセキュリティ侵害検出を試みてもよく(MAY)，そのようなセキュリティ侵害が検出された場合には操作を完了すべきでではない(SHOULD NOT)．

ClaimantとVerifierとの間の通信(アウトオブバンドAuthenticatorの場合はプライマリチャネル)は，Authenticator出力の秘匿性と中間者攻撃に対する耐性を提供するAuthenticateされた保護チャネルを介して行われるものとする(SHALL)．

政府機関が運用するVerifierは，AAL 1において，[FIPS 140](#FIPS140-2) Level 1 の要件に適合していることが確認されるものとする(SHALL)．

#### 4.1.3 Reauthentication
[Section 7.2](#sessionreauthn)に記載があるように，Subscriberのセッションの定期的なReauthenticationが行われるものとする(SHALL)．AAL 1では，ユーザの活動に関わらず，SubscriberのReauthenticationは少なくとも30日に1回は繰り返し実施すべきである(SHOULD)．セッションはこの時間制限に到達したら終了される(すなわちログアウトされる)べき(SHOULD)である

#### 4.1.4 セキュリティ統制
CSPは，[SP 800-53](#SP800-53) または等価な連邦政府機関(例えば [FEDRAMP](#FEDRAMP)) あるいは業界標準で定義されているセキュリティ統制の*低度な*基準から適切に調整されているセキュリティ統制を採用することとする(SHALL)．CSPは*影響が低度な*システム，またはそれに相当するものに対する最低限の保証関連の統制を果たしていることを保証することとする(SHALL)．

#### 4.1.5 レコード保持ポリシ
CSPは，準拠法，規則，及び任意のNational Archives and Records Administration (NARA)のレコード保持スケジュールを含むポリシに合致するそれぞれのレコード保持ポリシに従う．もしCSPがいずれの必須要件なくレコードを保持することを選択する場合，CSPはレコードの保持期間を決定するためのプライバシ及びセキュリティリスクのアセスメントを含むリスク管理プロセスを実施するものとし(SHALL)，Subscriberに対して当該保持ポリシについて通知するものとする(SHALL)．

### 4.2 Authenticator Assurance Level 2
*本セクションは標準である．*

AAL 2は，Subscriberのアカウントに対して結び付けられているAuthenticatorをClaimantが制御しているという，高い確実性を提供する．セキュアなAuthenticationプロトコルを介して，2つの異なるAuthentication要素の所有と制御の証明を必要とする．Approved Cryptographic技術がAAL2及びそれ以上では必要である．

#### 4.2.1 許可されているAuthenticatorタイプ
AAL2のAuthenticationでは， 一つの多要素Authenticatorまたは2つの単一要素Authenticatorの組み合わせのどちらかを利用するものとする(SHALL)．一つの多要素Authenticatorは，デバイスをアクティベートするために統合されたバイオメトリックセンサを備え，暗号的にセキュアであるようなデバイスのように，1回のAuthenticationイベントで二つの要素を必要とする．Authenticatorの要件は[Section 5](#sec5)で指定されている．

多要素Authenticatorを利用する際，以下の任意のものを利用してよい(MAY): 

* 多要素 OTP デバイス ([Section 5.1.5](#multifactorOTP))
* 多要素暗号ソフトウェア ([Section 5.1.8](#mfcs))
* 多要素暗号デバイス ([Section 5.1.9](#mfcd))

2つの単一要素Authenticatorを組み合わせる際には，記憶シークレットAuthentiactor ([Section 5.1.1](#memsecret)) と以下のリストから1つの所有ベース("something you have")のAuthenticatorを含むこととする(SHALL):

* ルックアップシークレット ([Section 5.1.2](#lookupsecrets))
* アウトオブバンドデバイス  ([Section 5.1.3](#out-of-band))
* 単一要素 OTP デバイス ([Section 5.1.4](#singlefactorOTP))
* 単一要素暗号ソフトウェア ([Section 5.1.6](#sfcs))
* 単一要素暗号デバイス ([Section 5.1.7](#sfcd))

> 注記: [Section 5.2.3](#biometric_use)でのバイオメトリックAuthenticationの要件を満たすために，デバイスはバイオメトリックに加えて，デバイスをAuthenticateされる必要がある &mdash バイオメトリックは1つの要素としてみなせるが，それ自体をAuthenticatorとしてみなしてはいない．従って，バイオメトリックを用いたAuthenticationを実施する時，2つのAuthenticatorを使う必要はない．なぜならば，関連付けられたデバイスは"something you have"として機能し，バイオメトリックは"something you are"として機能するからである．

#### 4.2.2 Authenticator及びVerifierの要件
AAL 2で用いられる暗号Authenticatorは，Approved Cryptographyを使うものとする(SHALL)．政府機関によって調達されたAuthenticatorは，[FIPS 140](#FIPS140-2) Level 1の要件に適合していることを確認されるものとする(SHALL)．オペレーティング・システム環境で動作するソフトウェアベースのAuthenticatorは，該当する場合(例，マルウェアによって)それ自身が動作しているプラットフォームのセキュリティ侵害検知を試みてもよく(MAY)，そのようなセキュリティ侵害が検出されると操作を拒否すべき(SHOULD)である．AAL2では少なくとも1つのAuthenticatorは[Section 5.2.8](#replay)に記載されているように，リプレイ耐性があるものとする(SHALL)．AAL2のAuthenticatorは[Section 5.2.9](#intent)に記載されているように，少なくとも1つのAuthenticatorからAuthenticationの意図を明示するべきである(SHOULD)．

ClaimantとVerifierとの間の通信(アウトオブバンドAuthenticatorの場合はプライマリチャネル)は，Authenticator出力の秘匿性と中間者攻撃に対する耐性を提供するAuthenticateされた保護チャネルを介して行われるものとする(SHALL)．

政府機関が運用するVerifierは，AAL 2において，[FIPS 140](#FIPS140-2) Level 1 の要件に適合していることが確認されるものとする(SHALL)．

Authenticationプロセスでスマートフォンなどのデバイスが利用される際に，（典型的にはPINやバイオメトリックを利用して）デバイスをアンロックする行為はAuthentication要素の一つとしてみなされないものとする(SHALL NOT)．一般的には，Verifierがそのデバイスがロックされていたのか，あるいはアンロックプロセスが関連するAuthenticatorタイプの要件に合致していたのかを把握することができない．

バイオメトリック要素がAAL 2で利用される場合，[Section 5.2.3](#biometric_use)に規定されたパフォーマンス要件を満たすものとし(SHALL)，Verifierはバイオメトリックセンサー及び後続処理がこれらの要件に合致していることを明確にしなければならない(SHOULD)．

#### 4.2.3 Reauthentication
[Section 7.2](#sessionreauthn)に記載があるように，Subscriberのセッションの定期的なReauthenticationが行われるものとする(SHALL)．AAL 2では，ユーザの活動に関わらず，SubscriberのReauthenticationはセッションの利用が延長されている間は少なくとも12時間に1回は繰り返されるものとする(SHALL)．
30分以上継続している非活動期間の後は，SubscriberのReauthenticationは繰り返されるものとする(SHALL)．
セッションはこの時間制限に到達したら終了される(すなわちログアウトされる)ものとする(SHALL)．

まだ時間制限に到達していないセッションのReauthenticationは，記憶シークレットだけ，あるいはまだ有効なセッションシークレットと連動しているバイオメトリックを要求してもよい(MAY)．Verifierは非活動によるタイムアウトの直前に，ユーザに対して活動を促してもよい(MAY)．

#### 4.2.4 セキュリティ統制
CSPは，[SP 800-53](#SP800-53) または等価な連邦政府機関(例えば [FEDRAMP](#FEDRAMP)) あるいは業界標準で定義されているセキュリティ統制の*中度な*基準から適切に調整されているセキュリティ統制を採用することとする(SHALL)．CSPは*影響が中度な*システム，またはそれに相当するものに対する最低限の保証関連の統制を果たしていることを保証することとする(SHALL)．

#### 4.2.5 レコード保持ポリシ
CSPは，準拠法，規則，及び任意のNational Archives and Records Administration (NARA)のレコード保持スケジュールを含むポリシに合致するそれぞれのレコード保持ポリシに従う．もしCSPがいずれの必須要件なくレコードを保持することを選択する場合，CSPはレコードの保持期間を決定するためのプライバシ及びセキュリティリスクのアセスメントを含むリスク管理プロセスを実施するものとし(SHALL)，Subscriberに対して当該保持ポリシについて通知するものとする(SHALL)．

### 4.3 Authenticator Assurance Level 3
*本セクションは標準である．*

AAL 3は，Subscriberのアカウントに対して結び付けられているAuthenticatorをClaimantが制御しているという，極めて高い確実性を提供する．AAL3におけるAuthenticationは，暗号プロトコルを介した，鍵の所有の証明に基づいている．．AAL3 Authenticationは，1つのハードウェアベースのAuthenticatorと，1つのVerifierなりすまし耐性を備えるAuthenticatorを利用するものとする(SHALL)．同じデバイスが両方の要件を満たしても良い(MAY)．AAL3でAuthenticateするために，claimantはセキュアなAuthenticationプロトコルを介して，2つの異なるAuthentication要素の所有と制御を証明するものとする(SHALL)．Approved Cryptographic技術が必要である．

#### 4.3.1 許可されているAuthenticatorタイプ
AAL3のAuthenticationでは， セクション4.3の要件を満たすAuthenticatorの組み合わせの1つを利用するものとする(SHALL)．有効な組み合わせは次の通り:．

* 多要素暗号デバイス ([Section 5.1.9](#mfcd))
* 単一要素暗号デバイス ([Section 5.1.7](#sfcd))を記憶シークレット ([Section 5.1.1](#memsecret))と併用
* 多要素 OTP デバイス(ソフトウェアまたはハードウェア) ([Section 5.1.5](#multifactorOTP))を単一要素暗号デバイス ([Section 5.1.7](#sfcd))と併用
* 多要素 OTP デバイス(ハードウェアのみ) ([Section 5.1.5](#multifactorOTP))を単一要素暗号ソフトウェア ([Section 5.1.6](#sfcs))と併用
* 単一要素 OTP デバイス(ハードウェアのみ) ([Section 5.1.4](#singlefactorOTP)) を多要素暗号ソフトウェア ([Section 5.1.8](#mfcs))と併用
* 単一要素 OTP デバイス(ハードウェアのみ) ([Section 5.1.4](#singlefactorOTP)) を単一要素暗号ソフトウェア ([Section 5.1.6](#sfcs))及び記憶シークレット([Section 5.1.1](#memsecret))と併用

#### 4.3.2 Authenticator及びVerifierの要件
ClaimantとVerifierとの間の通信は，Authenticator出力の秘匿性と中間者攻撃に対する耐性を提供するAuthenticateされた保護チャネルを介して行われるものとする(SHALL)．AAL3 Authenticationで利用される全ての暗号デバイスAuthenticatorは，[Section 5.2.5](#verifimpers)に記載されているVerifierなりすまし耐性を備えているものとし(SHALL)，[Section 5.2.8](#replay)に記載されているように，リプレイ耐性があるものとする(SHALL)．AAL3のAuthenticatorは[Section 5.2.9](#intent)に記載されているように，少なくとも1つのAuthenticatorからAuthentication及びReauthenticationの意図を明示するべきである(SHOULD)．

AAL 3で利用される多要素Authenticatorは，少なくとも[FIPS 140](#FIPS140-2) Level 3物理セキュリティを伴っており，総合的に[FIPS 140](#FIPS140-2) Level 2またはそれ以上で確認されたハードウェア暗号モジュールであるものとする(SHALL)．AAL3で利用される単一要素暗号デバイスは，少なくとも[FIPS 140](#FIPS140-2) Level 3物理セキュリティを伴っており，総合的に[FIPS 140](#FIPS140-2) Level 1またはそれ以上で確認されたハードウェア暗号モジュールであるものとする(SHALL)．

AAL3におけるVerifierは，[FIPS 140](#FIPS140-2) Level 1またはそれ以上の基準で確認されるものとする(SHALL)．

AAL3におけるVerifierは，[Section 5.2.7](#verifier-secrets)に記載のあるように，少なくとも１つのAuthenticator要素においてVerifier危殆化耐性があるものとする(SHALL)．

AAL3におけるハードウェアベースのAuthenticatorとVerifierは，関連するサイドチャネル(例，タイミング及び消費電力分析)攻撃への耐性があるべきである(SHOULD)．関連するサイドチャネル攻撃は，CSPによって実施されるリスクアセスメントにより，明確にされるものとする(SHALL)．

Authenticationプロセスでスマートフォンなどのデバイスが利用される際に，デバイスが上記の要件に合致している可能性が推測される場合でも，デバイスをアンロックする行為はAuthentication要素の一つとしてみなされないものとする(SHALL NOT)．これは，一般的には，Verifierがそのデバイスがロックされていたのか，あるいはアンロックプロセスが関連するAuthenticatorタイプの要件に合致していたのかを把握することができないからである．

バイオメトリック要素がAAL3で利用される場合，Verifierはバイオメトリックセンサー及び後続処理が，[Section 5.2.3](#biometric_use)に規定されたパフォーマンス要件に合致していることを明確にしなければならない(SHALL)．

#### 4.3.3 Reauthentication
[Section 7.2](#sessionreauthn)に記載があるように，Subscriberのセッションの定期的なReauthenticationが行われるものとする(SHALL)．AAL3では，[Section 7.2](#sessionreauthn)に記載があるように，ユーザの活動に関わらず，SubscriberのReauthenticationはセッションの利用が延長されている間は少なくとも12時間に1回は繰り返されるものとする(SHALL)．15分以上継続している非活動期間の後は，SubscriberのReauthenticationは繰り返されるものとする(SHALL)．Reauthenticationには両方のAuthenticator要素を用いるものとする(SHALL)．セッションはこれらの時間制限に到達したら終了される(すなわちログアウトされる)ものとする(SHALL)．Verifierは非活動によるタイムアウトの直前に，ユーザに対して活動を促してもよい(MAY)．

#### 4.3.4 セキュリティ統制
CSPは，[SP 800-53](#SP800-53) または等価な連邦政府機関(例えば [FEDRAMP](#FEDRAMP)) あるいは業界標準で定義されているセキュリティ統制の*高度な*基準から適切に調整されているセキュリティ統制を採用することとする(SHALL)．CSPは*影響が高度な*システム，またはそれに相当するものに対する最低限の保証関連の統制を果たしていることを保証することとする(SHALL)．

#### 4.3.5 レコード保持ポリシ
CSPは，準拠法，規則，及び任意のNational Archives and Records Administration (NARA)のレコード保持スケジュールを含むポリシに合致するそれぞれのレコード保持ポリシに従う．もしCSPがいずれの必須要件なくレコードを保持することを選択する場合，CSPはレコードの保持期間を決定するためのプライバシ及びセキュリティリスクのアセスメントを含むリスク管理プロセスを実施するものとし(SHALL)，Subscriberに対して当該保持ポリシについて通知するものとする(SHALL)．

### 4.4 プライバシ要件
CSPは[SP 800-53](#SP800-53)で定義された適切に調整されたプライバシコントロール，または他の等価な業界標準を採用するものとする(SHALL)．

CSPは，追加の用途のために明確な通知を行い加入者から承諾を得なければ，Authentication実施，詐欺緩和，または法令遵守や法的手続き以外のいかなる目的でもSubscriberの情報を利用・開示しないものとする(SHALL NOT)．CSPは，同意をサービスの条件としてはならない(SHALL NOT)．情報を収集した際の元々の目的に利用が限定されていることを保証するための対策が講じられるものとする(SHALL)．このような情報の利用が，Authenticationや法令遵守，法的手続きに関連する用途に該当しないのであれば，CSPは通知を行い，Subscriberから同意を得るものとする(SHALL)．この通知は，[SP 800-63A](sp800-63a.html) Section 8.2の*Notice and Consent*に記載されているのと同じ原則に従うべき(SHOULD)であり，法律に固執し過ぎたプライバシポリシや一般的な利用規約に丸め込むべきではない(SHOULD NOT)．明示的な目的外の用途がある場合は，むしろSubscriberが追加の利用目的を理解するための意味のある方法，及び承諾または辞退する機会が提供されるべきである(SHOULD)．

CSPが政府機関または民間のプロバイダであるかどうかにかかわらず，次の要件がAuthenticationサービスを提供・利用する政府機関に対して適用される:

* 政府機関は，Authenticatorの発行・維持を目的としたPIIの収集が*Privacy Act of 1974* [[Privacy Act]](#PrivacyAct) ([Section 9.4](#agency-privacy)参照)の要件をもたらすかどうか明確にするため，各機関のSenior Agency Official for Privacy (SAOP)と協議するものとする(SHALL)．
* 政府機関は，必要に応じてそのような収集活動を対象とするSystem of Records Notice (SORN)を公開するものとする(SHALL)．
* 政府機関は，Authenticatorの発行・維持を目的としたPIIの収集が*E-Government Act of 2002* [[E-Gov]](#E-Gov)の要件をもたらすかどうか明確にするため，各機関のSAOPと協議するものとする(SHALL)．
* 政府機関は，必要に応じてそのような収集活動を対象とするPrivacy Impact Assement(PIA)を公開するものとする(SHALL)．

### 4.5 要件サマリ
*本セクションは参考情報である*

Table 4-1にて各AAL毎の要件のサマリを述べる:

**Table 4-1 各AAL毎の要件サマリ**

要件 | AAL1 | AAL2 | AAL3
------------|-------|-------|-------
**許可されているAuthenticatorタイプ** | 記憶シークレット;<br />ルックアップシークレット;<br />アウトオブバンド;<br />単一要素OTPデバイス;<br />多要素OTPデバイス;<br />単一要素暗号ソフトウェア;<br />単一要素暗号デバイス;<br />多要素暗号ソフトウェア;<br />多要素暗号デバイス<br /> | 多要素OTPデバイス;<br />多要素暗号ソフトウェア;<br />単一要素暗号デバイス;<br />または 記憶シークレット及び:<br />&nbsp;&bull;&nbsp;ルックアップシークレット<br />&nbsp;&bull;&nbsp;アウトオブバンド<br />&nbsp;&bull;&nbsp;単一要素OTPデバイス<br />&nbsp;&bull;&nbsp;単一要素暗号ソフトウェア<br />&nbsp;&bull;&nbsp;単一要素暗号デバイス<br /> | 多要素暗号デバイス;<br />単一要素暗号デバイス及び &nbsp;&nbsp;記憶シークレット;<br />単一要素OTPデバイス及び多要素暗号デバイスまたはソフトウェア;<br />単一要素OTPデバイス及び単一要素暗号ソフトウェア及び記憶シークレット
**FIPS 140 確認** | Level 1 (政府機関のVerifier) | Level 1 (政府機関のAuthenticator及びVerifier) | Level 2 総合 (多要素Authenticator)<br />Level 1 総合 (Verifier及び単一要素暗号デバイス)<br />Level 3 物理セキュリティ (全てのAuthenticator)
**Reauthentication** | 30 日 | 12 時間 または 30 分 の非活動; 1つのAuthentication要素でもよい(MAY) | 12 時間 または 15 分 の非活動; 両方のAuthentication要素をつかうものとする(SHALL)
**セキュリティ統制**|[SP 800-53](#SP800-53) 低度のベースライン(または等価)|[SP 800-53](#SP800-53) 中度のベースライン(または等価)|[SP 800-53](#SP800-53) 高度のベースライン(または等価)
**中間者攻撃耐性** | 必須 | 必須 | 必須 |
**Verifierなりすまし耐性** | 不要 | 不要 | 必須 |
**Verifier危殆化耐性** | 不要 | 不要 | 必須 |
**リプレイ耐性** | 不要 | 必須 | 必須 |
**Authentication意図** | 不要 | 推奨 | 必須 |
**レコード保持ポリシ** | 必須 | 必須 | 必須 |
**プライバシ統制** | 必須 | 必須 | 必須 |

## 5 認証器及び検証者要件
_本セクションは標準である．_

本セクションでは，各Authenticatorタイプごとの要件詳細について記載する．[Section 4](#AAL_SEC4)で指定されたReauthentication要件及び[Section 5.2.5]に記載があるAAL3におけるVerifierなりすまし耐性の例外を除いて，各Authenticatorタイプ毎の技術要件はAuthenticatorが利用されるAALに関わらず同様である．

### 5.1 Authenticatorタイプ毎の要件

#### 5.1.1 記憶シークレット

<div class="text-left" markdown="1">
<table style="width:100%">
  <tr>
    <td><img src="media/Memorized-secret.png" alt="authenticator" style="width: 100px;height: 100px;min-width:100px;min-height:100px;"/></td>
    <td>記憶シークレットAuthenticatorは、一般的には<i>パスワード</i>や，数字ならば<i>PIN</i>として表現されているものであり，ユーザによって選択され，記憶されるシークレットである．記憶シークレットは攻撃者が正しい値を推測したり秘密の値を特定できないように，十分に複雑かつ秘密にしておく必要がある． 記憶シークレットは <i>something you know</i> である．</td>
  </tr>
  </table>
  </div>

#### 5.1.1.1 記憶シークレット認証器
記憶シークレットは，Subscriberにより選択されば場合少なくとも8文字とするものとする(SHALL)．記憶シークレットがCSPまたはVerifierによってランダムに選択されたものである場合は，少なくとも6文字であるものとし(SHALL)，全て数字でもよい(MAY)．
CSPやVerifierが，危殆化した値のブラックリストに出現状況に基づいて指定された記憶シークレットを拒否した場合，Subscriberは別の記憶シークレット値を選ぶよう要求されるものとする(SHALL)．記憶シークレットの複雑さに関する他の要件を課すべきではない(SHOULD)．本件についての論拠は[Appendix A](#appA)の _Strength of Memorized Secrets_ に記載されている．

#### 5.1.1.2 記憶シークレット検証者
Verifierは，Subscriberが選択した記憶シークレットに対して最低8文字であることを要求するものとする(SHALL)．Verifierは，Subscriberが選択した記憶シークレットに対して最低64文字を許可すべきである(SHOULD)．すべての印字可能なASCII [[RFC 20]](#RFC20) 文字(スペースも同様)は記憶シークレットとして許容されるべきである(SHOULD)．Unicode[[ISO/ISC 10646]](#ISOIEC10646)文字も同様に許容されるべきである(SHOULD)．Verifierは，8文字以上であることの検証を行う前に，タイピングミスの類を考慮して連続した複数のスペースまたは全てのスペースを除去してもよい(MAY)．シークレットの切り詰めについては実施しないものとする(SHALL NOT)．前述の記憶シークレットの長さ要件を満たすために，それぞれのUnicodeの符号位置は一文字としてカウントされるものとする(SHALL)．

もしUnicode文字が記憶シークレットとして許容されるならば，Verifierは，Unicode Standard Annex 15 [[UAX 15]](#UAX15) の Section 12.1で定義されている"Stablized Strings"のためのNFKCまたはNFKD正規化のいずれかを用いた正規化処理を適用すべきである(SHOULD)．この処理は記憶シークレットのバイト文字表現のハッシュ化の前に適用される．Unicode文字を含む記憶シークレットを選択したSubscriberは，いくつかのエンドポイントでは異なる表現になるかもしれない文字があり，それが彼らが正しくAuthenticationを行う能力に影響する可能性があるということをことを通知されるべきである(SHOULD)．

記憶シークレットは，CSP(例えばEnrollment時など)やVerifier(ユーザが新しいPINを要求した時など)によりランダムに選択されるもので，最低6文字であるものとし(SHALL)，Approve済み乱数生成器 [[SP 800-90Ar1]](#SP800-90Ar1) を利用して生成されるものとする(SHALL)．

記憶シークレットVerifierは，Subscriberに対して，UnauthenticatedでないClaimantでも到達可能な「ヒント」を記録することを許可しないものとする(SHALL NOT)．
記憶シークレットを選択する際，VerifierはSubscriberに対して特定のタイプの情報（例えば，「あなたが飼った最初のペットの名前はなんですか？」といったもの）の入力を求めないものとする(SHALL NOT)．

記憶シークレットの設定，変更の要求を処理する際，Verifierは候補となっているシークレットの値を，一般的に利用されている値，予想されうる値，危殆化した値として知られている値を含むリストに対し，比較するものとする(SHALL)．例えば，以下のリストが含んでいるものでよい(MAY)が，限定するものではない:

* 過去に漏洩した語彙集から得られるパスワード
* 辞書に含まれる言葉
* 繰り返しまたはシーケンシャルな文字 (例： 'aaaaaa'，'1234abcd')
* サービス名や，ユーザ名，そこから派生するようなものなど，文脈で特定可能な単語

もし選択したシークレットがリスト中に存在したら，CSPまたはVerifierはSubscriberに対して異なるシークレット値を選ぶ必要があるということを知らせるものとし(SHALL)，その理由を提供するものとし(SHALL)，そしてSubscriberに異なる値を選択するよう求められるものとする(SHALL)．

VerifierはSubscriberに対して，ユーザが強力な記憶シークレットを選択するのを支援するために，パスワード強度メーター [[Meters]](#meters) のようなガイダンスを行うべきである(SHOULD)．上記におリストに載っている記憶シークレットを拒否したのち，[[Blacklists]](#blacklists)化されている(そしておそらく非常に弱い)記憶シークレットからのささいな変更を思いとどまらせることは特に重要である．

Verifierは，[Section 5.2.2](#throttle)に記載されているように，SubscriberのアカウントにおけるAuthentication失敗回数を効果的に制限するレート制限の仕組みを実装するものとする(SHALL)．

Verifierは他の構成ルール(例えば，異なる文字種の組み合わせ，一定の文字の繰り返し)を記憶シークレットに課すべきではない(SHOULD NOT)．Verifierは，記憶シークレットを任意で(例えば，定期的に)変更するよう要求すべきではない(SHOULD NOT)．しかしながらAuthenticatorが危殆化した証拠がある場合は，変更を強制するものとする(SHALL)．

VerifierはClaimantによる記憶シークレットを入力時に"ペースト"機能を利用することを許可すべきである(SHOULD)．これはパスワードマネージャの利用を促進し，広く利用されるようになることで多くの場合ユーザがより強力な記憶シークレットを選択する可能性を増加させる．

Claimantが記憶シークレットを正しく入力することを支援するために，Verifierは，ドットやアスタリスク表示ではなく，入力され終わるまでシークレットを表示するオプションを提供すべき(SHOULD)である．こうすることで，Claimantは彼らのスクリーンが盗み見られる可能性が低い場所にいる場合に，入力内容を検証することができる．Verifierは更に，ユーザのデバイスに対して，入力が正しいことを確認する目的で個々の文字を入力後に短期間表示することを許可してもよい(MAY)．これは特にモバイルデバイスに当てはまる．

VerifierはApprove済み暗号化を利用するものとし(SHALL)，記憶シークレットを要求する際には，盗聴や中間者攻撃を防止するためにAuthenticateされた保護チャネルを用いるものとする(SHALL)．

Verifierは，オフライン攻撃に対して耐性をもった形式で記憶シークレットを保存するものとする(SHALL)．シークレットは， ソルトを追加したうえで適切な一方向の鍵導出関数を利用してハッシュされるものとする(SHALL)，鍵導出関数は，パスワード，ソルト，コストファクタを入力して，パスワードハッシュを生成する．例えば[[SP800-132]](#SP800-132)で記載されているPBKDF2のようなApprove済みハッシュを用いてハッシュ化されるものとする(SHALL)．ソルト値は32ビット以上のランダム値で，承認済み(approved)の乱数生成器を用いて生成され，ハッシュ結果とともに記録される．少なくとも繰り返し10000回のハッシュ関数を適用すべきである(SHOULD)．ハッシュAuthenticatorから分離されて記録される鍵(例:ハードウェアセキュリティモジュール中)を用いる鍵付ハッシュ関数(例:HMAC)は，記録済みハッシュ化Authenticatorに対する辞書攻撃に対する更なる対抗方法として利用されるべきである(SHOULD)．パスワードハッシュのファイルを入手した攻撃者によってパスワード推測の試行に費用がかかるようにする．そのためパスワード推測攻撃のコストは高い，もしくは非常に法外なものになる．適切な鍵導出関数の例としてPassword-based Key Derivation Function 2 (PBKDF2) [[SP 800-132]](#SP800-132)及びBalloon [[BALLOON]](#balloon)がある．攻撃コストを増加させることができるため，memory-hardな関数が利用されるべきである(SHOULD)．鍵導出関数はApprove済み一方向関数が用いられるべき(SHOULD)であり，例えば，Keyed Hash Message Authentication Code (HMAC) [[FIPS 198-1]](#FIPS198-1)，[SP 800-107](#SP800-107)のApprove済みハッシュ関数の何れか，Secure Hash Algorithm 3 (SHA-3) [[FIPS 202]](#FIPS202)，CMAC [[SP 800-38B]](#SP800-38B)，Keccak Message Authentication Code (KMAC)，Customizable SHAKE (cSHAKE)，ParallelHash [[SP 800-185]](#SP800-185)などがある．鍵導出関数からの出力を選択する際，根拠となる一方向関数の出力と長さが同じであるべきである(SHOULD)．

ソルトは少なくとも32ビットの長さで，保存されたハッシュ間でソルト値の衝突が最小化されるように任意に選択されたものとする(SHALL)．ソルト値及び結果のハッシュはいずれも記憶シークレットAuthenticatorを利用してSubscriber毎に保存されるものとする(SHALL)．

PBKDF2において，コストファクタは繰り返し回数である: PBKDF2関数が繰り返し適用される回数が増加すれば，パスワードハッシュの計算時間が必要となる．すなわち，繰り返し回数は検証サーバのパフォーマンスが許す限り大きくするべきであり(SHOULD)，概ね10,000以上とすべきである．

加えて，Verifierは，自身だけが知っているシークレットをソルト値として用いた鍵導出関数の適用を追加で実施すべきである(SHOULD)．もし可能ならばソルト値はApprove済み乱数生成器 [[SP 800-90Ar1]](#SP800-90Ar1)を利用して生成されるべき(SHOULD)であり，[SP 800-131A](#SP800-131A)の最新版で指定されている最小のセキュリティ強度(本書の公開日時点では112ビット)を少なくとも備えるべき(SHOULD)である．このシークレットソルト値は，ハッシュ化された記憶シークレットとは別に(例えば，ハードウェアセキュリティモジュールなどの特別なデバイスの中に)保存されるものとする(SHALL)．この追加の適用により，シークレットソルト値が秘密である限り，記憶シークレットのハッシュに対するブルートフォース攻撃は非現実的である．

#### <a name="lookupsecrets"></a> 5.1.2 ルックアップシークレット

<div class="text-left" markdown="1">
<table style="width:100%">
  <tr>
    <td><img src="media/Look-up-secrets.png" alt="authenticator" style="width: 100px;height: 100px;min-width:100px;min-height:100px;"/></td>
    <td>ルックアップシークレットAuthenticatorは物理的または電子的なレコードであり，ClaimantとCSPとの間で共有されているシークレット一式を記録するものである．Claimantは，Verifierからの入力要求に答えるために必要とされる適切なシークレットを検索するためにAuthenticatorを利用する．例えば，VerifierはClaimantに対して，カード上に印字された表形式の数字または文字列のうち特定の一部を提示するよう求められるかもしれない．ルックアップシークレットの一般的な適用としては，Subscriberによって保存され，別のAuthenticatorが紛失したり機能しなくなった際に用いる"リカバリキー"の利用がある．ルックアップシークレットは<i>something you have</i>である．</td>
  </tr>
  </table>
  </div>

#### 5.1.2.1 ルックアップシークレットAuthenticator
CSPがルックアップシークレットAuthenticatorを生成する際，Approve済み乱数生成器 [[SP 800-90Ar1]](#SP800-90Ar1) を用いてシークレットのリストを生成するものとし(SHALL)，Subscriberに対してAuthenticatorを安全に届けるものとする(SHALL)．ルックアップシークレットは最低20ビットのエントロピーを持つものとする(SHALL)．

ルックアップシークレットは，CSPの人間の手での配布，Subscriberの住所宛への配送，またはオンラインでの配布が行われてもよい(MAY)，もし配布がオンラインで行われるならば，ルックアップシークレットはpost-enrollment binding要件[Section 6.1.2](#post-enroll-bind)を満たすセキュアチャネル上で配布されるものとする(SHALL)．

もしAuthenticatorがルックアップシークレットを連続してリストから利用する場合，Subscriberは一度Authenticationに成功した場合に限ってシークレットを破棄してもよい(MAY)．

#### 5.1.2.2 Look-Up Secret Verifiers

ルックアップシークレットのVerifierはClaimantに対して，彼らのAuthenticatorから得られる次のシークレット，または特定の(例:付番された)シークレットの入力を促すものとする(SHALL)．Authenticatorから得られたシークレットは1度しか正常に利用できないものとする(SHALL)．もしルックアップシークレットが格子状のカードから得られる場合，格子の各セルは1度だけ利用するものとする(SHALL)．

Verifierは，オフライン攻撃へ対策する形式でルックアップシークレットを保存するものとする(SHALL)．112ビット以上のエントロピーを持ったルックアップシークレットは，[Section 5.1.1.2](#memsecretver)に記載されているようにApprove済み一方向関数でハッシュ化されるものとする(SHALL)．112ビット未満のエントロピーを持ったルックアップシークレットは[Section 5.1.1.2](#memsecretver)に記載されているように，ソルトを追加したうえで適切な一方向の鍵導出関数を用いてハッシュされるものとする．ソルト値は32ビット以上の長さで，保存されたハッシュ間でソルト値の衝突が最小化されるように任意に選択されるものとする(SHALL)．ソルト値及び結果のハッシュはいずれもルックアップシークレット毎に保存されるものとする(SHALL)．

64ビット未満のエントロピーを持つルックアップシークレットに対して，Verifierは，[Section 5.2.2](#throttle)に記載されているように，SubscriberのアカウントにおけるAuthentication失敗回数を効果的に制限するレート制限の仕組みを実装するものとする(SHALL)．

VerifierはApprove済み暗号化を利用するものとし(SHALL)，ルックアップシークレットを要求する際には，盗聴や中間者攻撃を防止する目的で，Authenticateされた保護チャネルを利用するものとする(SHALL)．

#### 5.1.3 アウトオブバンドデバイス
<div class="text-left" markdown="1">
<table style="width:100%">
  <tr>
    <td><img src="media/Out-of-band-OOB.png" alt="authenticator" style="width: 100px;height: 100px;min-width:100px;min-height:100px;"/></td>
    <td>アウトオブバンドAuthenticatorは，一意にアドレス可能，かつセカンダリチャネルと呼ばれる異なる通信チャネルを介してVerifierと安全に通信することができる物理デバイスである．デバイスはClaimantによって所有及び制御されており，E-Authenticationのためのプライマリチャネルと分離されたセカンダリチャネルを介したプライベートな通信をサポートしている．アウトオブバンドAuthenticatorは<i>something you have</i>である．<br><br>アウトオブバンドAuthenticatorは以下の方法の1つで動作する: <br><br>- Claimantはアウトオブバンドデバイスによってセカンダリチャネルを介して受け取ったシークレットを，プライマリチャネルを使ってVerifierに送信する．例えば，Claimantは自身のモバイルデバイス上でシークレットを受信し，それ(一般的には数字6桁のコード)を自身のAuthenticationセッションに対して入力してもよい．<br><br>- Claimantはプライマリチャネルを介して受け取ったシークレットを，アウトオブバンドデバイスに対して送信し，セカンダリチャネルを介してVerifierに対して送信する．例えば，Claimantは自身のAuthenticationセッション上で確認したシークレットを，モバイルデバイス上のアプリケーション対して入力したり，バーコード・QRコードといった技術を利用して送信を行ってもよい．<br><br>- Claimantはプライマリチャネルとセカンダリチャネルから得たシークレットを比較し，セカンダリチャネルを介したAuthenticationを確認する．<br><br>シークレットの目的は安全にAuthentication操作をプライマリとセカンダリのチャネルに結びつけることである．プライマリ通信チャネルを介してレスポンスをする場合，シークレットはアウトオブバンドデバイスをClaimantが制御していることもまた証明していることになる．</td>
  </tr>
  </table>
  </div>

#### 5.1.3.1 アウトオブバンドAuthenticator
アウトオブバンドAuthenticatorはアウトオブバンドシークレットやAuthenticationリクエストを取得するためにVerifierとの分離された通信チャネルを確立するものとする(SHALL)．このチャネルはプライマリ通信チャネルとの関係においてアウトオブバンドであるとし，(同じデバイス上で終端されている場合でさえも)，Claimantの認可なしに一方から他方に対して情報が漏洩することがないようなデイバイスであることを前提とする．

アウトオブバンドデバイスは一意にアドレス可能であるべきであり(SHOULD)，セカンダリチャネルを介した通信は，公衆交換電話網(PSTN)を介して送信されない場合は暗号化されているものとする(SHALL)．PSTNに固有な追加のAuthenticator要件に対しては，[Section 5.1.3.3](#pstnOOB)を参照すること．Voice-over-IP(VoIP)やEmailなど，特定デバイスの所有を証明を行わない方式は，アウトオブバンドAuthenticationを行う目的では利用しないものとする(SHALL)．

アウトオブバンドAuthenticatorは，Verifierとの通信において以下に記載する方法の1つを用いて，一意に自身の真正性を証明するものとする(SHALL):

- Approve済み暗号理論を利用してVerifierに対するAuthenticateされた保護チャネルを確立すること．鍵はAuthenticatorアプリケーションが利用可能なデバイス上で適切にセキュアであるストレージ(例:キーチェーンストレージ，TPM，TEE，セキュアエレメント)に記録されるものとする(SHALL)．

- SIMカードまたはデバイスを一意に識別する等価な方法を用いて公衆携帯電話網に対してAuthenticateする．この方法はPSTN(SMSまたは音声)を介してアウトオブバンドデバイスに対してVerifierからシークレットが送信される場合に限り用いるものとする(SHALL)．

もしVerifierからアウトオブバンドデバイスに対してシークレットが送信されるならば，デバイスは所有者によってデバイスがロックされている(例:表示するために，PINやパスコード入力，またはバイオメトリックの入力が必要とされる)間はAuthenticationシークレットを表示すべきではない(SHOULD NOT)．しかしながらAuthenticatorはロックされたデバイス上でAuthenticationシークレットを受信したことを表示すべきである(SHOULD)．

もしアウトオブバンドAuthenticatorがセカンダリ通信チャネルを介して承認メッセージを送信するならば(Claimantがプライマリ通信チャネルに対して受け取ったシークレットを送信するよりもむしろ)，次のうち1つを実施するものとする(SHALL):

* Authenticatorはプライマリチャネルから得たシークレットを転送することを許容するものとし(SHALL)，Authenticationトランザクションに対する承認と関連付けるために，セカンダリチャネルを介してVerifierに対して送信するものとする(SHALL)．Claimantは送信を手動で実施，またはバーコード・QRコードといった技術を利用して送信を行ってもよい(MAY)．

* Authenticatorはセカンダリチャネルを介してVerifierから受け取ったシークレットを提示し，Claimantに対してプライマリチャネルのシークレットとの一貫性を検証するよう促した上で，Claimantから「はい/いいえ」の応答を受け入れるものとする(SHALL)．

#### 5.1.3.2 アウトオブバンドVerifier
PSTNに特有の追加の検証要件は，[Section 5.1.3.3](#pstnOOB)を参照すること．

アウトオブバンド検証がセキュアなアプリケーション(例:スマートフォン上)を利用して行われる場合，Verifierはデバイスに対してPush通知を行ってもよい(MAY)．VerifierはAuthenticateされた保護チャネルの確立を待ち，Authenticatorの識別キーを検証する．Authenticatorは識別キー自身を記録しないものとする(SHALL NOT)が，Authenticatorを一意に識別するためにハッシュのような検証方法(例えばApprove済みのハッシュ関数や，識別キーの所持証明)を用いるものとする(SHALL)．Authenticateされると，VerifierはAuthenticationシークレットをAuthenticatorに対して送信する．

アウトオブバンドAuthenticatorの種別に応じて，以下のうち1つを行うものとする(SHALL):

* シークレットをプライマリチャネルに送出: VerifierはSubscriberのAuthenticatorを持つデバイスに対してAuthenticationの準備を示すシグナルを送信してもよい(MAY)．そのうえで，アウトオブバンドAuthenticatorに対してランダムなシークレットを送信するものとする(SHALL)．Verifierはプライマリ通信チャネル上でシークレットが返却されるのを待つものとする(SHALL)．

* シークレットをセカンダリチャネルに送出: VerifierはランダムなAuthenticationシークレットをプライマリチャネル経由でClaimantに対して表示するものとする(SHALL)．そのうえで，セカンダリチャネル上でClaimantのアウトオブバンドAuthenticatorからシークレットが返却されるのを待つものとする(SHALL)．

* ClaimantによるシークレットのVerifier: VerifierはランダムなAuthenticationシークレットをプライマリチャネル経由でClaimantに対して表示するものとし(SHALL)，セカンダリチャネルを介してアウトオブバンドAuthenticatorに対して同じシークレットを送信しClaimantに提示するものとする(SHALL)．そのうえで，セカンダリチャネルを介した承認(非承認)メッセージを待つものとする(SHALL)．

全てのケースにおいて，10分以内に完了しないAuthenticationは不正とみなすものとする(SHALL)．[Section 5.2.8](#replay)に記載されているリプレイ耐性を備えるために，Verifierは特定のAuthenticationシークレットを確認期間の間で一度だけ受け付けるものとする(SHALL)．

Verifierは，Approve済み乱数生成器 [[SP 800-90Ar1]](#SP800-90Ar1) を用いて少なくとも20ビットのエントロピーでランダムなAuthenticationシークレットを生成するものとする(SHALL)．もし認証シークレットが64ビット未満のエントロピーを持つ場合は，ルックアップシークレットに対して，Verifierは，[Section 5.2.2](#throttle)に記載されているように，SubscriberのアカウントにおけるAuthentication失敗回数を効果的に制限するレート制限の仕組みを実装するものとする(SHALL)．

#### 5.1.3.3 公衆交換電話網を利用するAuthenticator
アウトオブバンドでの検証を目的としたPSTNの利用は，本セクション及び [Section 5.2.10](#restricted) に記載されているように制限されている(RESTRICTED)．もしアウトオブバンドでの検証がPSTNを用いて実施される場合は，Verifierは利用されている事前登録済みの電話番号が，特定の物理デバイスに結びつけられていることを検証するものとする(SHALL)．事前登録済みの電話番号の変更は，新しいAuthenticatorのバインディングとみなされ，[Section 6.1.2](#post-enroll-bind) に記載されている場合のみ発生するものとする(SHALL)．

Verifierは，デバイス入れ替え，SIM変更，番号ポーティング，あるいはその他にアウトオブバンドAuthenticationシークレットの配送にPSTNを利用する以前の異常な振る舞いのようなリスク指標を考慮すべきである(SHOULD)．

> 注記: [Section 5.2.10](#restricted)におけるAuthenticatorの制限に従い，NISTは脅威状況及びPSTNの技術的な運用の発展に基づき，PSTNの制限(RESTRICTED)状態を時間の経過とともに調整するかもしれない．

#### 5.1.4 単一要素OTPVerifier
<div class="text-left" markdown="1">
<table style="width:100%">
  <tr>
    <td><img src="media/Single-factor-otp-device.png" alt="authenticator" style="width: 100px;height: 100px;min-width:100px;min-height:100px;"/></td>
    <td>単一要素OTPデバイスはOTPの生成をサポートするデバイスである．単一要素OTPデバイスは，携帯電話のようなデバイスにインストールされたソフトウェアのOTP生成器及びハードウェアデバイスが含まれる．これらのデバイスは，OTPの生成のシードとして利用される組み込みのシークレットを保持しており，二要素目によるアクティベーションを必要としない．OTPはデバイス上で表示，手動でVerifierに対して入力され，そのことによりデバイスの所持と制御を証明する．OTPデバイスは例えば一度に6文字の表示を行うことがある．単一要素OTPデバイスは<i>something you have</i>である．<br><br>単一要素OTPデバイスはルックアップシークレットAuthenticatorと同様，暗号理論に基づいてAuthenticatorとVerifierがシークレットを生成し，Verifierによって比較されるという例外を除いて同様である．シークレットは，時間ベースのノンスまたはAuthenticatorとVerifierのカウンタに基づいて計算される.</td>
  </tr>
  </table>
  </div>

#### 5.1.4.1 Single-Factor OTP Authenticators
単一要素OTPAuthenticatorは2つの永続的な値を保持する.1つ目はデバイスの存続期間にわたり保持し続ける対象鍵である.2つ目は認証機が使われる都度変化する，またはリアルタイムクロックに基づいているノンスである.

秘密鍵とそのアルゴリズムは少なくとも[SP 800-131A](#SP800-131A)の最新版で定義された最低のセキュリティ強度(本書の刊行現在では112ビット)であるものとする(SHALL)．ノンスは，デバイスの存続期間にわたりデバイスが操作される都度一意な値であることを確実にするために十分長いこととする(SHALL). OTP Authenticatorは - 特にソフトウェアベースのOTP生成器の場合 - 複数デバイスに対して秘密鍵を複製する行為を思いとどまらせるべきであり(SHOULD)，助長しないものとする(SHALL NOT)．

Authenticator出力は，鍵とノンスとを安全な方法で組み合わせ，Approveされたブロック暗号またはハッシュ関数を用いることで得られる．Authenticator出力は少なくとも(約20ビットのエントロピーの)6桁の数字になるよう切り詰めてもよい(MAY).

もしAuthenticator出力を生成するために利用されるノンスがリアルタイムクロックをベースとしている場合，ノンスは少なくとも2分毎に変化するものとする(SHALL).与えられたノンスと関連するOTP値は一度だけ受け入れられるものとする(SHALL).

#### 5.1.4.2 単一要素OTP Verifier
単一要素OTP Verifierは，AuthenticatorによるOTPの生成プロセスを実質的に再現する．Verifierは，Authenticatorが使う対称鍵を所持しており，セキュリティ侵害に対する強力な防御が行われているものとする(SHALL)．

単一要素OTP AuthenticatorがSubscriberアカウントに紐付けられている時，Verifierまたは関連するCSPは，Authenticatorの出力を再現するために必要なシークレットを生成，交換または取得するためにApproveされた暗号理論を利用するものとする(SHALL)．

Verifierは，OTPを収集する際の盗聴や中間者攻撃に対抗するために，Approveされた暗号化を利用し，Authenticateされた保護チャネルを利用するものとする(SHALL)．時間ベースのOTP[[RFC 6238]](#RFC6238)は，有効期間を定義するものとし(SHALL)，Authenticatorの存続期間にわたり予想される（何れの方向に対する）時刻ずれにネットワーク遅延とユーザによるOTP入力の許容時間を加えて決定される．[Section 5.2.8](#replay)に記載されているリプレイ耐性を備えるために，Verifierは指定された時間ベースのOTPを有効期間で一度だけ受け付けるものとする(SHALL)．

もしAuthenticator出力が64ビット未満のエントロピーを持つ場合は，Verifierは，[Section 5.2.2](#throttle)に記載されているように，SubscriberのアカウントにおけるAuthentication失敗回数を効果的に制限するレート制限の仕組みを実装するものとする(SHALL)．

#### 5.1.5 多要素 OTPデバイス
<div class="text-left" markdown="1">
<table style="width:100%">
  <tr>
    <td><img src="media/Multi-factor-otp-device.png" alt="authenticator" style="width: 100px;height: 100px;min-width:100px;min-height:100px;"/></td>
    <td>多要素OTPデバイスは，追加の認証要素によりアクティベートされた後にAuthenticationで使われるワンタイムパスワードを生成する．多要素OTPデバイスは，ハードウェアデバイス及び携帯電話のようなデバイスにインストールされたソフトウェアのOTP生成器が含まれる．Authenticationの2要素目は組み込みの入力パッド，組み込みのバイオメトリック(例：指紋)リーダ，USBポートなどのコンピュータに対するダイレクトなインタフェースを介して実現される．ワンタイムパスワードはデバイス上で表示，手動でVerifierに対して入力され，そのことによりデバイスの所持と制御を証明する．ワンタイムパスワードデバイスは例えば一度に6文字の表示を行うことがある．多要素OTPデバイスは<i>something you have</i>である．また，<i>something you know</i>または<i>something you are</i>のどちらかによってアクティベートされるものとする(SHALL)．</td>
  </tr>
  </table>
  </div>


#### 5.1.5.1 多要素OTP Authenticator
多要素OTP Authenticatorは，Authenticatorからパスワードを得るために記憶シークレットの入力またはバイオメトリクスの利用を要求する点を除いて，単一要素OTP Authenticator([Section 5.1.4.1](#sfotpa)参照)と同じ方法で動作する．いずれの場合でも追加要素の入力が必要であるものとする(SHALL)．

アクティベーション情報に加えて，多要素OTP Authenticatorは2つの永続的な値を含んでいる．1つ目はデバイスの存続期間にわたり永続する対称鍵である．2つ目は，Authenticator利用の都度変化する，またはリアルタイムクロックに基づいているノンスである．

秘密鍵とそのアルゴリズムは少なくとも[[SP 800-131A]](#SP800-131A)の最新版で定義された最低のセキュリティ強度(本書の刊行現在では112ビット)であるものとする(SHALL)．ノンスは，デバイスの生存期間に渡りデバイスが操作される都度一意な値であることを確実にするために十分長いこととする(SHALL). OTP Authenticatorは - 特にソフトウェアベースのOTP生成器の場合 - 複数デバイスに対して秘密鍵を複製する行為を思いとどまらせるべきであり(SHOULD)，助長しないものとする(SHALL NOT)．

Authenticator出力は，鍵とノンスとを安全な方法で組み合わせ，Approveされたブロック暗号またはハッシュ関数を用いることで得られる．Authenticator出力は少なくとも(約20ビットのエントロピーの)6桁の数字になるよう切り詰めてもよい(MAY)．

もしAuthenticator出力を生成するために利用されるノンスがリアルタイムクロックをベースとしている場合，ノンスは少なくとも2分毎に変化するものとする(SHALL)．与えられたノンスと関連するOTP値は一度だけ受け入れられるものとする(SHALL)．

Authenticatorのアクティベーションのために利用される記憶シークレットは，ランダムに選ばれた最低6文字の数字または[Section 5.1.1.2](#memsecretver)に記載されているものと同等な複雑さを備えた他の記憶シークレットであるものとし(SHALL)，[Section 5.2.2](#throttle)に記載されているようにレート制限が行われるものとする(SHALL)．バイオメトリックのアクティベーション要素は，[Section 5.2.3](#biometric_use)の要件を満たすものとし(SHALL)，連続するAuthentication失敗回数に制限がある．

暗号化されていない秘密鍵とアクティベーションシークレット，またはバイオメトリック標本（及び信号処理により生成されたプローブのようなバイオメトリック標本に由来する任意のバイオメトリックデータ）は，パスワード生成が終わると直ちにゼロ埋めされるものとする(SHALL)．

#### 5.1.5.2 多要素OTP Verifier
多要素OTP Verifierは，AuthenticatorによるOTPの生成プロセスを実質的に再現するが，2要素目の要求は行わない．例えば，Authenticatorが用いる対象鍵は，セキュリティ侵害に対して強力な防御が行われているものとする(SHALL)．

多要素OTP AuthenticatorがSubscriberアカウントに紐付けられている時，Verifierまたは関連するCSPは，Authenticatorの出力を再現するために必要なシークレットを生成，交換または取得するためにApproveされた暗号理論を利用するものとする(SHALL)．VerifierまたはCSPは，Authenticatorの出所によって，Authenticatorが多要素デバイスであることを証明するものとする(SHALL)．それが多要素Authenticatorであるという信頼できる報告がない場合、Verifierは[Section 5.1.4](#singlefactorOTP)に従いAuthenticatorを単一要素として扱うものとする(SHALL)．

Verifierは，OTPを収集する際の盗聴や中間者攻撃に対抗するために，Approveされた暗号化を利用し，Authenticateされた保護チャネルを利用するものとする(SHALL)．時間ベースのOTP[[RFC 6238]](#RFC6238)は，有効期間を定義するものとし(SHALL)，Authenticator自体の寿命までに予想される（何れの方向に対する）時刻ずれにネットワーク遅延とユーザによるOTP入力の許容時間を加えて決定される．[Section 5.2.8](#replay)に記載されているリプレイ耐性を備えるために，Verifierは指定された時間ベースのOTPを有効期間で一度だけ受け付けるものとする(SHALL)．あるOTPの使い回しによりClaimantのAuthenticationが拒否されるような場合には，VerifierはClaimantに対して，攻撃者が先回りしてAuthenticationを行うことができる可能性があることを警告してもよい(MAY)．Verifierは，あるOTPの使い回しが試みられた既存セッションのSubscriberに対して同様に警告してもよい(MAY)．

もしAuthenticator出力またはアクティベーションシークレットが64ビット未満のエントロピーを持つ場合は，Verifierは，[Section 5.2.2](#throttle)に記載されているように，SubscriberのアカウントにおけるAuthentication失敗回数を効果的に制限するレート制限の仕組みを実装するものとする(SHALL)．バイオメトリックのアクティベーション要素は，[Section 5.2.3](#biometric_use)の要件を満たすものとし(SHALL)，連続するAuthentication失敗回数に制限がある．

#### 5.1.6 単一要素暗号ソフトウェア
<div class="text-left" markdown="1">
<table style="width:100%">
  <tr>
    <td><img src="media/Single-factor-software-crypto.png" alt="authenticator" style="width: 100px;height: 100px;min-width:100px;min-height:100px;"/></td>
    <td>単一要素ソフトウェア暗号Authenticatorは、ディスクあるいは"ソフト"媒体に記録された暗号鍵である．Authenticationは鍵の所有と制御を証明することで行われる．Authenticator出力は特定の暗号プロトコルに強く依存し、一般的にはある種の署名付メッセージになっている．単一要素ソフトウェア暗号Authenticatorは<i>something you have</i>である．</td>
  </tr>
  </table>
  </div>


#### 5.1.6.1 単一要素暗号ソフトウェアAuthenticator
単一要素暗号ソフトウェアAuthenticatorは、Authenticatorで一意な秘密鍵を保持している．鍵はAuthenticatorアプリケーションが利用可能なデバイス上で適切にセキュアであるストレージ(例:キーチェーンストレージ，TPMまたは可能であればTEE)に記録されるものとする(SHALL)．デバイス上のソフトウェアコンポーネントがアクセス要求を行ったときのみ鍵にアクセスできるよう制限するアクセス制御を用いて、鍵は許可のない暴露から強力に保護されているものとする(SHALL)．単一要素暗号ソフトウェアAuthenticatorは，複数デバイスに対して秘密鍵を複製する行為を思いとどまらせるべきであり(SHOULD)，助長しないものとする(SHALL NOT)．

#### 5.1.6.2 単一要素暗号ソフトウェアVerifier
単一要素暗号ソフトウェアVerifierに対する要件は，[Section 5.1.7.2](#sfcdv) に記載されている単一要素暗号デバイスVerifierに対する要件と同一である．

#### 5.1.7 単一要素暗号デバイス
<div class="text-left" markdown="1">
<table style="width:100%">
  <tr>
    <td><img src="media/Single-factor-crypto.png" alt="authenticator" style="width: 100px;height: 100px;min-width:100px;min-height:100px;"/></td>
    <td>単一要素暗号デバイスは，保護された暗号鍵を用いた暗号操作，及びユーザエンドポイントに対する直接コネクションを介してAuthenticator出力を提供するハードウェアデバイスである．デバイスは組み込みの対象暗号鍵，非対称暗号鍵を利用し，Authenticationの2要素目を用いたアクティベーションを要求しない．AuthenticationはAuthenticationプロトコルを介してデバイスの所持証明を行うことにより達成される．Authenticator出力はユーザエンドポイントに対する直接接続により提供され，特定の暗号デバイスとプロトコルに強く依存し，典型的にはある種の署名付メッセージになっている．単一要素暗号デバイスは<i>something you have</i>である．</td>
  </tr>
  </table>
  </div>

#### 5.1.7.1 単一要素暗号デバイスAuthenticator
単一要素暗号デバイスAuthenticatorは秘密鍵を格納しており，その鍵はデバイスで一意かつエクスポートされない(つまりデバイスから除去することができない)ものとする(SHALL NOT)．Authenticatorは，通常はUSBポートなどのコンピュータに対するダイレクトなインタフェースを介して提供されるチャレンジノンスに署名することで動作する．暗号デバイスはソフトウェアを含んでいるが，全ての組み込みソフトウェアがCSP(または発行者)の制御下にあるという点，及びAuthenticator全体でAuthentication時のAALにおけるFIPS 140要件に適合する必要がある点において，暗号ソフトウェアAuthenticatorとは異なる．

秘密鍵とそのアルゴリズムは少なくとも[SP 800-131A](#SP800-131A)の最新版で定義された最低のセキュリティ強度(本書の刊行現在では112ビット)であるものとする(SHALL)．チャレンジノンスは少なくとも64ビット長であるものとする(SHALL)．Approveされた暗号理論が利用されるものとする(SHALL)．

単一要素暗号デバイスAuthenticatorは動作するために(ボタンを押すなどの)物理的な入力を必要とすべきである(SHOULD)．これにより，デバイスが接続している先がセキュリティ侵害をうけているような場合に起こりうる，デバイスの意図しない動作を防止することができる．

#### 5.1.7.2 単一要素暗号デバイスVerifier
単一要素暗号デバイスVerifierはチャレンジノンスを生成して，対応するAuthenticatorに送信する．また，Authenticator出力をデバイスの所有を検証するために用いる．Authenticator出力は特定の暗号デバイスとプロトコルに強く依存し，典型的にはある種の署名付メッセージになっている．
Verifierは，各Authenticatorに対応する対象鍵または非対称暗号鍵を保持している．どちらのタイプの鍵も改変に対して保護されているものとし(SHALL)，対象鍵については追加で許可のない暴露に対して保護されているものとする(SHALL)．

チャレンジノンスは少なくとも64ビット長であるとし(SHALL)，Authenticatorの有効期間を通じて一意，または統計上一意(したがって，Approveされた乱数生成器[[SP 800-90Ar1]](#SP800-90Ar1)を用いて生成されたもの)であるとする(SHALL)．

#### 5.1.8 多要素暗号ソフトウェア
<div class="text-left" markdown="1">
<table style="width:100%">
  <tr>
    <td><img src="media/Multi-factor-software-crypto.png" alt="authenticator" style="width: 100px;height: 100px;min-width:100px;min-height:100px;"/></td>
    <td>多要素ソフトウェア暗号Authenticatorは，ディスクあるいは"ソフト"媒体に記録された暗号鍵であり，Authenticationの2要素目を用いたアクティベーションを必要とする．Authenticationは鍵の所有と制御を証明することで行われる．Authenticator出力は特定の暗号プロトコルに強く依存し，一般的にはある種の署名付メッセージになっている．多要素ソフトウェア暗号Authenticatorは<i>something you have</i>であり，<i>something you know</i>または<i>something you are</i>のどちらかによってアクティベートされるものとする(SHALL)．</td>
  </tr>
  </table>
  </div>

#### 5.1.8.1 多要素暗号ソフトウェアAuthenticators
多要素暗号ソフトウェアAuthenticatorは秘密鍵を格納しており，その鍵はデバイスで一意であり記憶シークレットやバイオメトリックなどの追加の要素の入力を経たときだけアクセスできる．鍵はAuthenticatorアプリケーションが利用可能なデバイス上で適切にセキュアであるストレージ(例:キーチェーンストレージ，TPM，TEE)に記録されるべきである(SHOULD)．デバイス上のソフトウェアコンポーネントがアクセス要求を行ったときのみ鍵にアクセスできるよう制限するアクセス制御を用いて、鍵は許可のない暴露から強力に保護されているものとする(SHALL)．多要素暗号ソフトウェアAuthenticatorは，複数デバイスに対して秘密鍵を複製する行為を思いとどまらせるべきであり(SHOULD)，助長しないものとする(SHALL NOT)．

Authenticatorを用いた各Authentication操作には両方の要素の入力を必要とするものとする(SHALL)．

Authenticatorのアクティベーションのために利用される記憶シークレットは，最低6文字の数字または同等な複雑さを備えたものとし(SHALL)，[Section 5.2.2](#throttle)に記載されているようにレート制限が行われるものとする(SHALL)．バイオメトリックのアクティベーション要素は，[Section 5.2.3](#biometric_use)の要件を満たすものとし(SHALL)，連続するAuthentication失敗回数に対する制限を含むものとする．

暗号化されていない秘密鍵とアクティベーションシークレット，またはバイオメトリック標本（及び信号処理により生成されたプローブのようなバイオメトリック標本に由来する任意のバイオメトリックデータ）は，Authenticationトランザクションが行われたら直ちにゼロ埋めされるものとする(SHALL)．

#### 5.1.8.2 多要素暗号ソフトウェアVerifier
多要素暗号ソフトウェアVerifierに対する要件は，[Section 5.1.7.2](#sfcdv) に記載されている単一要素暗号デバイスVerifierに対する要件と同一である．
多要素暗号ソフトウェアAuthenticator出力を検証することで，アクティベーション要素の使用の証明となる．

#### 5.1.9 多要素暗号デバイス
<div class="text-left" markdown="1">
<table style="width:100%">
  <tr>
    <td><img src="media/Multi-factor-crypto-device.png" alt="authenticator" style="width: 100px;height: 100px;min-width:100px;min-height:100px;"/></td>
    <td>多要素暗号デバイスは，1つ以上の保護された暗号鍵を用いた暗号操作を行うハードウェアデバイスであり，2要素目を用いたアクティベーションを要求する．Authenticationはデバイスの所持と鍵の制御を証明することで実現される．Authenticator出力はユーザエンドポイントに対する直接接続を介して提供され，特定の暗号デバイスとプロトコルに強く依存し，典型的にはある種の署名付メッセージになっている．多要素暗号デバイスは<i>something you have</i>であり，<i>something you know</i>または<i>something you are</i>のどちらかによってアクティベートされるものとする(SHALL)．</td>
  </tr>
  </table>
  </div>

#### 5.1.9.1 多要素暗号デバイスAuthenticator
多要素暗号デバイスAuthenticatorは耐タンパ性を有するハードウェアを用いて秘密鍵を格納しており，その鍵はデバイスで一意であり記憶シークレットやバイオメトリックなどの追加の要素の入力を経たときだけアクセスできる．鍵はAuthenticatorアプリケーションが利用可能なデバイス上で適切にセキュアであるストレージ(例:キーチェーンストレージ，TPM，TEE)に記録されるべきである(SHOULD)．デバイス上のソフトウェアコンポーネントがアクセス要求を行ったときのみ鍵にアクセスできるよう制限するアクセス制御を用いて、鍵は許可のない暴露から強力に保護されているものとする(SHALL)．多要素暗号ソフトウェアAuthenticatorは，複数デバイスに対して秘密鍵を複製する行為を思いとどまらせるべきであり(SHOULD)，助長しないものとする(SHALL NOT)．

秘密鍵とそのアルゴリズムは少なくとも[SP 800-131A](#SP800-131A)の最新版で定義された最低のセキュリティ強度(本書の刊行現在では112ビット)であるものとする(SHALL)．チャレンジノンスは少なくとも64ビット長であるものとする(SHALL)．Approveされた暗号理論が利用されるものとする(SHALL)．

Authenticatorを用いた各Authentication操作には追加の要素の入力を必要とすべきである(SOULD)．追加要素の入力は，デバイスへの直接入力かハードウェア接続(例：USB，スマートカード)を介して実施されてもよい(MAY)．

Authenticatorのアクティベーションのために利用される記憶シークレットは，最低6文字の数字または同等な複雑さを備えたものとし(SHALL)，[Section 5.2.2](#throttle)に記載されているようにレート制限が行われるものとする(SHALL)．バイオメトリックのアクティベーション要素は，[Section 5.2.3](#biometric_use)の要件を満たすものとし(SHALL)，連続するAuthentication失敗回数に対する制限を含むものとする．

暗号化されていない秘密鍵とアクティベーションシークレット，またはバイオメトリック標本（及び信号処理により生成されたプローブのようなバイオメトリック標本に由来する任意のバイオメトリックデータ）は，Authenticationトランザクションが行われたら直ちにメモリが上書きされるものとする(SHALL)．

#### 5.1.9.2多要素暗号デバイスVerifier
多要素暗号デバイスVerifierに対する要件は，[Section 5.1.7.2](#sfcdv) に記載されている単一要素暗号デバイスVerifierに対する要件と同一である．
多要素暗号デバイスAuthenticator出力を検証することで，アクティベーション要素の使用の証明となる．

#### 5.2. 一般Authenticator要件

#### 5.2.1 Physical Authenticators
CSPはSubscriberに対して，Authenticatorの盗難や紛失を正しく防止する方法について指示するものとする(SHALL)．CSPはSubscriberから認証機の盗難や紛失の疑いがある旨の通知に対し，直ちにAuthenticatorを無効化，停止するメカニズムを備えるものとする(SHALL)．

#### 5.2.2 レート制限 (スロットリング)
[Section 5.1](#reqauthtype)のAuthenticatorタイプの説明で要求される場合，Verifierはオンライン推測攻撃に対抗するための制御を実装するものとする(SHALL)．指定されたAuthenticatorの説明に記載がなければ，Verifierはオンライン攻撃者に対し，同一アカウントで100回以上の連続した認証失敗試行を制限をするものとする(SHALL)．

レート制限の結果として攻撃者が正しいclaimantをロックアウトさせる頻度を減少させるために用いられる追加のテクニックを用いてもよい(MAY)．追加テクニックは以下を含む:

- Claimantに対して，Authentication試行前にCAPTCHAの入力を要求する．

- Claimantに対して認証失敗後に一定期間待つように要求し，連続する認証失敗の最大回数に近づくにつれてその時間を(例えば30秒から1時間まで)増加させる．

- Subscriberが以前Authenticationに成功したことがあるIPアドレスのホワイトリストからのみ行われるAuthentication要求だけを受理する．

- ユーザの振る舞いが通常の範疇にあるかないかを特定するリスクベースまたは適応型Authenticationの手法を活用する．

SubscriberがAuthenticationに成功した場合，Verifierは同一IPアドレスからの以前の失敗したAuthenticationの試行を無視すべきである(SHOULD)．

#### 5.2.3 バイオメトリクスの利用
Authenticationにおけるバイオメトリクス(*something you are*)の利用は，物理的な特性(例：指紋，虹彩，顔の特徴)及び振る舞い特性(例：タイピングのリズム)の両方を測定法を含んでいる．[Section 5.2.9](#intent)に記載されているAuthentication意図を確認する範囲とは差があるかもしれないが，両方の分類ともに，異なるバイオメトリック計測手段とみなされる．

様々な理由で，本書はAuthenticationにおけるバイオメトリクスの利用を限定的にサポートする．それらは以下のとおりである:

- バイオメトリックのFalse Match Rate(FMR)は，それ自身ではSubscriberのAuthenticationにおける確実性を与えるものではない．更に，FMRはスプーフィング攻撃を考慮したものではない．
- バイオメトリックの比較は確率的なものであるが，他のAuthentication要素は決定的なものである．
- バイオメトリックのテンプレート保護スキームは，他のAuthentication要素(例: PKI証明書やパスワード)に相当するバイオメトリッククレデンシャルを無効化するための手段を提供する．しかしながら，そのようなソリューションの可用性は制限されており，これらの手段を試験するための標準も策定中の段階である．
- バイオメトリック特性はシークレットにはならない．それらはオンラインで取得したり，知識のあるなしに関わらず誰かが携帯電話のカメラで(例えば顔の)写真を取ることができ，誰かが触った物から(例えば潜在的に指紋を)採取することができ，高精細な画像から(例えば虹彩パターンを)キャプチャすることができる．生体検知のようなPresentation attack detaction(PAD)技術は，これらの種別の攻撃リスクを緩和することができるが，センサーやバイオメトリック処理はCSPとSubscriberのニーズに合うようにPADを適切に実施していることを保証する必要がある．

したがって，Authenticationにおける制限されたバイオメトリクスの利用は，以下の要件とガイドラインの下でサポートされる:

バイオメトリクスは物理的なAuthenticatorを用いた多要素Authentication(*something you have*)の一部としてのみ利用されるものとする(SHALL)．

センサ(またはセンサ置き換え耐性のあるセンサーを持ったエンドポイント)とVerifier間のAuthenticateされた保護チャネルが確立され(SHALL)，センサーまたはエンドポイントが設置され(SHALL)，Claimantからバイオメトリック標本を取得するのに先立ってセンサーまたはエンドポイントがAuthenticateされているものとする(SHALL)．

バイオメトリックシステムは，FMR [[ISO/IEC 2382-37]](#ISOIEC2382-37)は1000分の1より優れたレートで運用されるものとする．このFMRは[[ISO/IEC 30107-1]](#ISOIEC30107-1)で定義されているConformant attack(すなわちZero-effort imporster attempt)の条件下で達成されるものとする(SHALL)．

バイオメトリックシステムは，PADを実装すべきである(SHOULD)．バイオメトリックシステムを配備を目的として実施するテストでは，各関連攻撃タイプ(すなわち種類)に対して少なくとも90%の耐性があることを示すべきである(SHOULD)．ここでの耐性はプレゼンテーション攻撃の失敗回数をプレゼンテーション攻撃の試行回数で割ったものとして定義される．プレゼンテーション攻撃耐性のテストは，[[ISO/IEC 30107-3]](#ISOIEC30107-3)の12条に従い行われるものとする(SHALL)．PADはClaimantのデバイス上でローカルに実施されても，または中央のVerifierによって実施されてもよい(MAY)．

>注釈: PADはガイドラインの将来の版では必須要件としてみなされる．

バイオメトリックシステム5回の連続したAuthentication試行の失敗を許容するものとする(SHALL)．もしPADが上記の要件を満たして実装されている場合は，10回の連続したAuthentication試行の失敗を許容するものとする(SHALL)．一度上限に達すると，バイオメトリックAuthenticatorは以下の何れかであるものとする(SHALL): 

- 次回の試行までに少なくとも30秒の遅延を課し，試行回数が増える毎に指数関数的に遅延を増加させる(例えば，後続の失敗する試行の前に1分，その次の試行では2分)か，

- もし既に利用可能な代替手段があるのであれば，バイオメトリックユーザAuthenticationを無効化し，もう一つの要素(例えば，異なるバイオメトリック計測手段や，まだ要求されていない要素であればPIN/パスコード)の利用を試みる．

Verifierはセンサー及びポイントのパフォーマンス，一貫性，真正性について決定するものとする(SHALL)．この決定を行うために容認できる方法として以下があるが，限定はされていない:

* センサー及びエンドポイントのAuthentication．
* 承認済みの適格性認定機関による認定．
* [Section 5.2.4](#attestation)に記載されている署名済みメタデータの実効時照会(例: アテステーション)

バイオメトリックの比較はClaimantのデバイス上でローカルに実施されるか，中央のVerifierで実施される．中央のVerifierでの大規模な攻撃の可能性が増加しつつあるため，ローカルでの比較が好ましい．

もし比較が中央で実施される場合:

* Authenticationとしてのバイオメトリック利用はApproveされた暗号理論を用いて特定される一つ以上の特定デバイスに限定されるものとする(SHALL)．バイオメトリックがまだメインのAuthentication鍵をアンロックしていないのであれば，分離している鍵がデバイスの特定のために利用されるものとする(SHALL)．
* バイオメトリックの破棄は，[ISO/IEC 24745](#ISO24745)中のバイオメトリックテンプレート保護と呼ばれており，実装するものとする(SHALL)．
* 全てのバイオメトリクスの送信はAuthenticateされた保護チャネルを介して行われる．

Authenticationプロセス中で収集されたバイオメトリック標本は，ユーザの同意の下で，研究目的で比較アルゴリズムの学習に利用してもよい(MAY)．バイオメトリック標本及び信号処理により生成されたプローブのようなバイオメトリック標本に由来する任意のバイオメトリックデータは，学習や派生した研究データが得られた後は直ちにゼロ埋めされるものとする(SHALL)．

バイオメトリクスは，[SP 800-63A](sp800-63a.html)で記載されている全てのEnrollmentプロセスフェーズにおいて，Enrollmentの否認を防ぐため，また関与する同一個人であることを検証するために利用される．

#### 5.2.4 アテステーション
アテステーションは，直接接続されているAuthenticatorやAuthentication操作に参加するエンドポイントに関してVerifierに対して伝達される情報のことである．アテステーションにより伝達される情報は次を含んでもよい(MAY)が，限定はされていない:

* Authenticatorやエンドポイントの出所(例えば，製造元やサプライヤ認定など)，健康および一貫性．
* Authenticatorのセキュリティ機能
* バイオメトリックセンサーのセキュリティや性能特性
* センサー計測手段

このアテステーションが署名される場合，少なくとも[SP 800-131A](#SP800-131A)の最新版で定義された最低のセキュリティ強度(本書の刊行現在では112ビット)を備えるデジタル署名を用いて署名されるものとする(SHALL)．

アテステーション情報は，VerifierのリスクベースAuthenticationの判断の一部に利用してもよい(MAY)．

#### 5.2.5 Verifierなりすまし耐性
Verifierなりすまし攻撃は，しばしばフィッシング攻撃と呼ばれ，VerifierやRPになりすまして不用心なClaimantを騙して詐欺サイトに対してAuthenticateさせる試みである．SP 800-63の以前の版では，Verifierなりすまし攻撃に対するプロトコル耐性は，中間者攻撃への強い耐性と呼ばれていた．

Verifierなりすまし耐性のあるAuthenticationプロトコルは，Authenticateされた保護チャネルをVerifierとの間に確立するものとする(SHALL)．Authenticateされた保護チャネルを確立する際にネゴシエートされたチャネル識別子は強く，変更できない形で(例えば，Verifierが知っている公開鍵に対する，Claimantが制御している秘密鍵を用いて2つの値を署名することにより)Authenticator出力に結び付けられるものとする(SHALL)．Verifierは署名またはVerifierなりすまし耐性を確認するための情報を確認するものとする(SHALL)．このようにすることで，不正なVerifierが，実際のVerifierを表す証明書を得た場合でさえも，異なるAuthenticateされた保護チャネルでAuthenticationを再生することを防止できる．

Verifierなりすまし耐性が要求された場合に，それを確立するためにApproveされた暗号アルゴリズムが利用されるものとする(SHALL)．この目的で利用される鍵は，少なくとも[SP 800-131A](#SP800-131A)の最新版で定義された最低のセキュリティ強度(本書の刊行現在では112ビット)を備えるものとする(SHALL)．

Verifierなりすまし耐性のあるAuthenticationプロトコルの例としては，Client-authenticated TLSがある．クライアントは，ネゴシエーション済みの特定TLSコネクション対して一意なプロトコルから予めメッセージを受取り，そのメッセージと共にAuthenticator出力に対して署名を行う．

アウトオブバンドAuthenticatorやOTP Authenticatorのような，Authenticator出力を手動入力するようなAuthenticatorでは，手動入力ではAuthenticator出力を特定のAuthenticateされたセッションに結びつけていないため，Verifierなりすまし耐性があるとは考えないものとする(SHALL NOT)．中間者攻撃の場合には，不正なVerifierはOTP Authenticator出力をVerifierに対して再生し，Authenticationが成功してしまう．

#### 5.2.6 Verifier-CSP通信
VerifierとCSPが個別のエンティティである場合([SP 800-63-3 Figure 4-1](sp800-63-3.html#63Sec4-Figure1)の点線で示されているように)，VerifierとCSPの間の通信は，(Client-authenticated TLS接続のような)Approveされた暗号理論を利用する相互にAuthenticateされたセキュアチャネルを介して行われるものとする(SHALL)．

#### 5.2.7 Verifier危殆化耐性
いくつかのAuthenticatorタイプを利用に際しては，VerifierがAuthenticatorシークレットのコピーを記録する必要がある．例えば，OTP Authenticator([Section 5.1.4](#singlefactorOTP)で記載)では，VerifierがClaimantから送信された値と比較するために，個別にAuthenticator出力を生成する必要がある．Verifierが危殆化され，記録されているシークレットが窃取される可能性があるため，Verifierが永続的にAuthenticationに利用するシークレットを記録する必要のないAuthenticationプロトコルは，より強力であるとみなされ，ここでは*Verifier危殆化耐性*があるものとして記載されている．そのようなVerifierが，全ての攻撃に耐性があるのではないことに注意すること．Verifierは，特定のAuthenticator出力を常に受け付けるように操作されるなど，異なる方法で危殆化される可能性がある．

Verifier危殆化耐性は異なる方法で実現することができる．例えば:

- 暗号Authenticatorを利用して，Authenticatorが保持する秘密鍵に対応する公開鍵をVerifierに記録する．
- 期待するAuthenticator出力をハッシュ形式で記録する．この方式は，例えばルックアップシークレット([Section 5.1.2](#lookupsecrets)に記載)で用いることができる．

Verifier危殆化耐性を考慮し，Verifierによって記録される公開鍵は，Approveされた暗号アルゴリズムの利用と関連付けられているものとし(SHALL)，少なくとも[SP 800-131A](#SP800-131A)の最新版で定義された最低のセキュリティ強度(本書の刊行現在では112ビット)を備えるものとする(SHALL)．

他のVerifier危殆化耐性のあるシークレットは，Approveされた暗号アルゴリズムを利用するものとし(SHALL)，基礎となるシークレットは少なくとも[SP 800-131A](#SP800-131A)の最新版で定義された最低のセキュリティ強度(本書の刊行現在では112ビット)を備えるものとする(SHALL)．より複雑性の低いシークレット(例えば記憶シークレット)は，辞書の探索や全探索などを通してハッシュ処理を攻略できる可能性があるため，ハッシュされていてもVerifier危殆化耐性を持つものと満たさないものとする(SHALL NOT)．

#### 5.2.8 リプレイ耐性
Authenticationプロセスは，以前のAuthenticationメッセージを記録し再生することでAuthenticationを成功させることが困難であるならば，リプレイアタックに耐性を持つ．リプレイ耐性は，Authenticator出力が保護チャネルに入力されるまえに窃取される可能性があるため，Authenticateされた保護チャネルのリプレイ耐性に加えて施されるものである．ノンスやチャレンジをトランザクションの"新鮮さ"を示すために用いるプロトコルは，再生されるメッセージが適切なノンスや時系列データを含んでおらず，Verifierが容易に過去のプロトコルメッセージが再生されたことを検知しうるため，リプレイ耐性があるといえる．

リプレイ耐性のあるAuthenticatorの例として，OTPデバイス，暗号Authenticatorおよびルックアップシークレットがある．

反対に記憶シークレットはAuthenticator出力がシークレットそのものであり，各Authenticationで入力されるため，リプレイ耐性があるものとはみなさない．

#### 5.2.9 Authentication意図
Authenticationプロセスは，Subjectが明示的に各AuthenticationやReauthenticationの要求に応じる必要がある場合，意図を明らかにする．Authentication意図の目的は，直接接続された物理的なAuthenticator(例えば，多要素暗号デイバス)が，エンドポイント上でマルウェアなどによりSubjectが知らないうちに利用されることをより困難にすることである．Authentication意図はそのAuthenticatorそのものにより立証するものとする(SHALL)が，多要素暗号デバイスがそのAuthenticatorが利用されるエンドポイント上で，他のAuthentication要素を再入力することにより立証してもよい(MAY)．

Authentication意図は，いくつかの方法で立証してもよい(MAY)．Subjectの介入(例えばOTPデバイスから得たAuthenticator出力をClaimantが入力すること)を必要とするAuthenticationプロセスは意図を立証している．各AuthenticationやReauthentication操作に対して(例えばボタン押下や再挿入のような)ユーザアクションを必要とする暗号デバイスもまた意図を立証している．

バイオメトリクスのプレゼンテーションは，計測手段に応じてAuthentication意図を立証したりしなかったりする．指紋のプレゼンテーションは通常は意図を立証するが，一方カメラを用いたClaimantの顔の観測は，一般的にはそうならない．行動バイオメトリクスは，Claimant側で特定のアクションを常に要求されるわけではないので，同様にAuthentication意図を立証する可能性は低い．

#### 5.2.10 制限された(RESTRICTED) Authenticator
脅威の進化につれて，Authenticatorが攻撃に対抗する能力は一般的には減少する．一方で，いくつかのAuthenticatorの性能は改善するかもしれない &mdash; 例えば，根拠となる標準が改定されることで特定の攻撃に対抗する能力が増加する．

Authenticatorの性能におけるこれらの変更を考慮するために，NISTは追加の制限を，Authenticatorタイプや特定のクラスまたはAuthenticatorタイプのインスタンスに対して設けた．

制限された(RESTRICTED) Authenticatorを利用するには，実装した組織が，制限された(RESTRICTED) Authenticatorに関連するリスクを評価，理解，許容し，時間の経過とともにリスクが増加しうるということを認識する必要がある．組織の責任は，彼らのシステムおよび関連データにおけるリスクを許容できるレベルを決定し，過度なリスクを緩和する方法を定義することである．何れかの当事者に対するリスクが許容できないと組織が判断した時点で，Authenticatorは利用されないものとする(SHALL NOT)．

更に，Authenticationエラーのリスクは一般的には，実装した組織，Authentication結果に依存する組織，Subscriberを含む複数の当事者が負うものである．組織が制限された(RESTRICTED) Authenticatorを採用すると，Subscriberがそのリスクを十分に理解していない，またリスクを統制する能力が制限されている可能性があり，Subscriberは追加のリスクにさらされることになる．その為，CSPは次のことを実施することとする(SHALL)．

1. Subscriberに対して少なくとも1つの制限されていない代替Authenticatorを提示し，要求されるAALでAuthenticateできるようにする．

2. Subscriberが理解しやすいように，制限されているAuthenticatorのセキュリティリスクと，制限されていない代替Authenticatorが利用可能であることを通知する．

3. リスクアセスメントにおけるSubscriberに対する追加のリスクに対処する．

4. 将来的にある地点で制限された(RESTRICTED) Authenticatorが許容可能でなくなる可能性を踏まえて移行プランを立て，[digital identity acceptance statement](sp800-63-3.html#daps)の内容ににこの移行プランを含めておく．

## 6 認証器ライフサイクルの要件
*本セクションは標準である．*

SubscriberのAuthenticatorのライフサイクルにわたってAuthenticatorの利用に影響する様々なイベントが発生する．これらのイベントは，バインディング，紛失，盗難，不正な複製，有効期限切れ，破棄を含む．本性ではそれぞれのイベントで取るべきアクションについて述べる．

### 6.1 Authenticatorバインディング
*Authenticatorバインディング* は特定のSubscriberアカウントとAuthenticator間の関連を確立するものであり，Authenticatorを - 他のAuthenticatorと組み合わせる場合もあるが - そのアカウントをAuthenticateするために利用できる状態にする．

AuthenticatorはSubscriberアカウントに以下のいずれかによって結び付けられるものとする(SHALL): 

- Enrollmentの一部としてCSPにより発行される，または
- Subscriberが提示したCSPで利用するのに適切なAuthenticatorを関連付ける

これらのガイドラインでは， 双方の選択肢に対応するために，Authenticatorの発行よりもむしろ *バインディング* について述べる．

デジタルアイデンティティのライフサイクルを通じて，CSPは各アイデンティティと関連付けられる，または既に関連付けられている全てのAuthenticatorのレコードを保持する(SHALL)．CSPやVerifierは，[Section 5.2.2](#throttle)に記載されているように，Authentication試行のスロットリングに必要な情報を保持する(SHALL)．また，CSPはユーザが提示したAuthenticatorのタイプ(例えば，単一要素暗号デバイスであるのか，多要素暗号デバイスであるのか)を検証し(SHALL)，Verifierは各AALの要件に合致しているかどうかを明らかにすることができる．

CSPによって生成されるレコードは，Authenticatorがアカウントに結び付けられた日時を含むものとする(SHALL)．レコードは，Enrollmentに関連づけられている全てのデバイスのバインディングの発生源(例えばIPアドレス，デバイス識別子)に関する情報を含むべきである(SHOULD)．可能であれば，レコードは，AuthenticatorでのAuthentication試行が失敗した際の発生源についての情報についてもまた含むべきである(SHALL)．

何らかの新しいAuthenticatorがSubscriberアカウントに結び付けられる際，CSPは，バインディングプロトコルおよび関連する鍵のプロビジョニングを目的としたプロトコルが，そのAuthenticatorが使われるAALに見合ったセキュリティのレベルで，実施されたということを保証する(SHALL)．例えば，鍵のプロビジョニングを目的としたプロトコルがAuthenticateされた保護チャネルを利用して行われるものとする(SHALL)，または中間者攻撃を防御するため人間によって実施されるものとする(SHALL)．多要素Authenticatorのバインディングには，多要素Authenticationまたは等価(例えば，Identity proofingが実施された直後のセッションとの関連付け)なものがAuthenticatorをバインドするために必要である．Authenticatorが鍵ペアを生成して公開鍵をCSPに送信する際には，同じ条件が適用される．

#### 6.1.1 Enrollment時のバインディング
Authenticatorが[SP 800-63A](sp800-63a.html)に記載されているIdentity proofingのトランザクションが成功した結果として，アイデンティティにバインドされる時，次の要件が適用される．Executive Order 13681 [[EO 13681]](#EO13681)によりあらゆる個人データの開示には，多要素Authenticationを利用する必要があるため，AuthenticatorはSubscriberアカウントにEnrollment時にバインドされ，Identity proofingに確立されているものを含む個人データへのアクセスが許可されることが重要である．

CSPは，記憶シークレットまたは1つ以上のバイオメトリクスに加えて，Subscriberのオンラインアイデンティティに対する物理的な (*something you have*) Authenticatorを少なくとも1つはバインドし(SHALL)，2つ以上バインドすべきである(SHOULD)．

全ての識別情報はIAL1でself-assertedされているが，オンライン素材の保存やオンラインでの評判は，Authenticatorの紛失が原因でアカウントの制御を消失することを望ましくないようにする．二つ目のAuthenticatorはあるAuthenticatorの紛失から安全に復活させることを可能にする．この理由により，CSPは少なくともIAL1でも同様にSubscriberクレデンシャルに少なくとも2つの物理Authenticatorをバインドすべき(SHOULD)である．

IAL2以上では，識別情報は，デジタルアイデンティティに関連付けられ，Subscriberは[SP 800-63A](sp800-63a.html)に記載されているIdentity proofingプロセスを受ける．結果として，要求するIALと同じAALのAuthenticatorがアカウントにバインドされるものとする(SHALL)．例えば，もしSubscriberがIAL2でproofingに成功したならば，AAL2またはAAL3のAuthenticatorがIAL2のアイデンティティにバインドするために適切である．CSPはAAL1のAuthenticatorをIAL2のアイデンティティにバインドしてもよい(MAY)が，もしSubscriberがAAL1でAuthenticateされた場合には，CSPは，それが仮にself-assertedであっても，個人情報をSubscriberに対して開示しない(SHALL NOT)．以前の文章で述べているように，追加のAuthenticatorが利用可能であれば，あるAuthenticatorが破損，紛失，盗難してしまった場合に備える手段を提供することになる．

もし，一度の物理的接触や，電子的トランザクション(すなわち，単一保護セッション)でEnrollmentおよびバインディングを完了することができない場合には，プロセスを通じてApplicantとして振る舞う当事者であることを保証するために以下の方法が利用されるものとする(SHALL):

リモートトランザクションの場合:

1. Applicantは，新しいバインディングのトランザクションにおいて，以前のトランザクションの間に確立した，またはApplicantの電話番号，Emailアドレス，住所に送付された一時的なシークレットを提示することで，自分自身であることを識別する(SHALL)．

2. 長期間のAuthenticatorシークレットは，保護セッション中でApplicantに対して発行されたものに限るものとする(SHALL)．

対面トランザクションの場合:

1. Applicantは上記リモートトランザクション(1)で記載したシークレットを利用するか，以前の接触時に記録したバイオメトリックの利用を通じて，対面で自分自身であることを識別する(SHALL)．

2. 一時シークレットは再利用されないものとする(SHALL NOT)．

3. もしCSPが物理的なトランザクションの間に長期間のAuthenticatorシークレットを発行するならば，それは対面でApplicantに対して発行された物理デバイスに対して局所的にロードされる，またはアドレスレコードを確認する方法で配送されるものとする(SHALL)．

#### 6.1.2 Post-Enrollment Binding


#### 6.1.2.1 既存AALにおける追加Authenticatorのバインディング
記憶シークレットを除いて，CSPとVerifierは，使用する各要素の少なくとも2つ有効なAuthenticatorの保持をSubscriberに促すべきである(SHOULD)．例えば，普段は物理AuthenticatorとしてOTPデバイスを利用しているSubscriberは，物理Authenticatorの紛失，盗難，破損に備えて，ルックアップAuthenticatorの番号を発行したり，アウトオブバンドAuthenticatorとしてデバイスを登録してもよい(MAY)．記憶シークレットAuthenticatorの置き換えに関するより多くの情報は [Section 6.1.2.3](#replacement) を参照すること．

したがって，CSPはSubscriberアカウントに対して追加のAuthenticatorをバインドすることを許可すべきである(SHOULD)．新しいAuthenticatorを追加する前に，まずCSPはSubscriberに対して，新しいAuthenticatorが使用されるAAL(またはそれ以上のAAL)でAuthenticateするよう要求する(SHALL)．Authenticatorの追加後は，CSPはSubscriberに対して，新しいAuthenticatorをバインドしたトランザクションと独立した方法(例えば，Subscriberに以前関連付けられているアドレスへのEmail)で，通知を行うべきである(SHOULD)．CSPは，この方法でバインドできるAuthenticatorの数を制限してもよい(MAY)．

#### 6.1.2.2 単一要素アカウントに対する追加要素の追加
Subscriberアカウントに1つのAuthentication要素がバインドされていて(すなわちIAL1/AAL1)，異なるAuthentication要素の追加Authenticatorを追加する場合，Subscriberはアカウントを AAL2 にアップグレードするよう要求してもよい(MAY)．IALはIAL1のままである．

新しいAuthenticatorのバンディング前に，CSPはSubscriberに対してAAL1でAuthenticateするよう要求する(SHALL)．新しいAuthenticatorをバインドしたトランザクションと独立した方法(例えば，Subscriberに以前関連付けられているアドレスへのEmail)で，通知を行うべきである(SHOULD)．

#### 6.1.2.3 紛失したAuthentication要素の置き換え
Subscriberが多要素Authenticationに必要な全ての要素のAuthenticatorを失い，かつIdentity proofがIAL2またはIAL3で実施されていた場合，Subscriberは[SP 800-63A](sp800-63a.html)で記載されているIdentity proofingプロセスを再び実施する(SHALL)．もしCSPがオリジナルのproofingプロセスから証拠を得ることができるならば，[SP 800-63A](sp800-63a.html) Section 4.2に記載されているプライバシリスクアセスメントに従って，以前提供された証拠とClaimantとがバインドされていることを確認する簡易Proofingプロセスを利用してもよい(MAY)．CSPは．既存のアイデンティティへのバインディングを確認するために，Claimantに対して可能であれば残りの要素のAuthenticatorを用いてAuthenticateするよう要求する(SHALL)．IAL3におけるAuthentication要素の再確立は，対面または[SP 800-63A](sp800-63a.html) Section 5.3.3.2に記載された監督済みリモートプロセスを通じて実施し(SHALL)，オリジナルのProofingプロセスで収取したバイオメトリックを検証する(SHALL)．

CSPはSubscriberに対して，そのイベントを通知すべきである(SHOULD)．Proofingプロセスの一部として要求されているものと同じ通知でもよい(MAY)．

記憶シークレットの紛失(すなわち忘れてしまうこと)の置き換えは，非常に一般的であるがゆえに解決が難しい問題である．追加の "バックアップ" 記憶シークレットは，結局忘れる可能性があるために緩和策にはならない．もしバイオメトリックがアカウントにバインドされていれば，バイオメトリックと関連する物理Authenticatorが新しい記憶シークレットを設定するために利用されるべきである(SHOULD)．

アカウントにバイオメトリックがバインドされていない場合には，上記のre-proofingプロセスの代わりとして，CSPは，Subscriberの住所の一つに送信された確認コードとともに2つの物理Authenticatorを用いたAuthenticationを行い新しい記憶シークレットをバインドしてもよい(MAY)．確認コードは，少なくともランダムな6文字の半角英数字で構成され，乱数生成器 [[SP 800-90Ar1]](#SP800-90Ar1) を利用して生成されるものとする(SHALL)．それらは，住所に送付され，最大7日間有効であるものとする(SHALL)．しかし，U.S. Postal Serviceが直接配送できない範囲の住所に適用する場合，例外プロセスとして最大21日まで有効としてもよい(MAY)．物理的なメール以外の方法で送信された確認コードの有効期限は最大10分とする(SHALL)．

#### 6.1.3 Subscriberが提示するAuthenticatorとのバインディング
Subscriberは特定のAALのAuthenticationにふさわしいAuthenticatorを既に所持しているかもしれない．例えば，彼らはAAL2及びIAL1とみなせるソーシャル・ネットワーク・プロバイダから2要素Authenticatorを受領しており，そのクレデンシャルをIAL2を必要とするRPで利用したいとする．

現実的には大量のAuthenticatorを管理するSubscriberの負荷を軽減するために，CSPはSubscriberが提供するAuthenticatorの利用に対応するべきである(SHOULD)．これらのAuthenticatorのバインディングは，[Section 6.1.2.1](#post-enroll-bind)に記載されているように実施する(SHALL)．Authenticator強度がself-evidentでない(例えば，与えられたAuthenticatorタイプが単一要素か多要素か)場合は，CSPは実際により強いAuthenticatorが使用されていることが(例えば，Authenticatorの発行者や製造者の検証などにより)分かるようにならない限り，より弱いAuthenticationの利用を前提とすべきである(SHOULD)．

#### 6.1.4 更新
CSPは，既存のAuthenticatorの有効期限に到達する前に適切な時間を確保して，更新後のAuthenticatorをバインドすべきである(SHOULD)．このプロセスは，初期のAuthenticatorバインディングプロセス(例えば，アドレスレコードを確認するなど)に正確に従うべきである(SHOULD)．新しいAuthenticatorが正しく使用できるようになると，CSPはそれが置き換えているAuthenticatorを無効化してもよい(MAY)．

### 6.2 紛失，盗難，破損，不正な複製
危殆化したAuthenticatorには，盗難，紛失，不正な複製の対象となったものが含まれる．一般的に，紛失したAuthenticatorは盗難されたりAuthenticatorの正当なSubscriberではない誰かによって危殆化されたものであると仮定しなければならない．破損または故障したAuthenticatorもまた，Authenticatorシークレットが抽出されたいかなる可能性に対しても防御するために，侵害されたとみなされる．攻撃者に入手されるといった危殆化した形跡がなく，忘れてしまった記憶シークレットは顕著な例外である．

危殆化したAuthenticatorの中断，無効化，破棄は，その検出後に速やかに行われるべきである(SHOULD)．政府機関はこのプロセスに時間制限を設けるべきである(SHOULD)．

Authenticatorの紛失，盗難，破損の安全に報告することを促すために，CSPは，Subscriberに対してバックアップまたは代替Authenticatorを利用してCSPに対してAuthenticateする方法を提供すべきである(SHOULD)．このバックアップAuthenticatorは記憶シークレットか物理Authenticatorのいずれかとする(SHALL)．どちらが利用されてもよい(MAY)が，この報告を行うためには1つだけのAuthentication要素だけが必要である．あるいは，Subscriberは，Authenticate済み保護チャネルを確立し，Proofingプロセスにおいて収集された情報を検証してもよい(MAY)．CSPはアドレスレコード(すなわちEmail，電話，住所)を検証し，危殆化したと報告のあったAuthenticatorを中断してもよい(MAY)．中断は，Subscriberが正当な(すなわち，中断されていない) Authenticatorを用いて正しくCSPに対してAuthenticateすることができ，Authenticatorを再アクティベーションを要求する場合，再開できるものとする(SHALL)．CSPは中断されているAuthenticatorがもはや再アクティベートができなくなるまでの時間制限を設けてもよい(MAY)．

### 6.3 有効期限切れ
CSPは有効期限があるAuthenticatorを発行してもよい(MAY)．Authenticatorの有効期限が切れた際には，Authenticationに利用することはできない(SHALL NOT)．有効期限切れのAuthenticatorを用いてAuthenticationが試行された時には，CSPはSubscriberに対してこのAuthentication失敗は他の理由ではく有効期限によるものであることを指摘すべきである(SHOULD)．

CSPはAuthenticatorの有効期限切れまたは更新されたAuthenticatorを受領した後，できるだけ早くCSPによって署名された属性証明書を含む物理Authenticatorの引き渡しまたは破棄したことの証明をSubscriberに対して求める(SHALL)．

### 6.4 失効と解約
Authenticatorの失効は，特にPIV Authenticatorの文脈では解約と呼ばれることがあるが，AuthenticatorとCSPが保持するクレデンシャルとのバインディングを除去することを指す．

CSPは，オンラインアイデンティティが存在しなくなった(例えば，Subscriberが死亡，詐称Subscriberであることが判明した)とき，Subscriberに要求されたとき，またはCSPがSubscriberがもはや加入要件を満たさなくなったと判断したとき，速やかにAuthenticatorのバインディングを削除する(SHALL)．

CSPは，失効または解約の実施後できるだけ早くCSPによって署名された属性証明書を含む物理Authenticatorの引き渡しまたは破棄したことの証明をSubscriberに対して求める(SHALL)．

PIV Authenticatorの解約に関するさらなる要件は [FIPS 201](#FIPS201) にある．

## 7 セッション管理
*This section is normative.*

一度 Authentication イベントが行われると，Subscriber が複数のインタラクションをまたぐアプリケーションを Authentication イベントを繰り返す必要なく継続して使用し続けられることはしばしば望まれる． この要件は， Authentication イベントがネットワーク経由で調整したいくつかのコンポーネントやパーティを巻き込む Federation シナリオ ( [SP 800-63C](sp800-63c.html) で説明される) に特に当てはまる．

この振る舞いを容易にするため，* Session * は，ある Authentication イベントへの応答によって開始されてもよい (MAY) ．そしてその Session はそれが終了されるときまで維持される． Session は，活動がないときのタイムアウト，明示的なログアウトイベント，その他の理由など，様々な理由により終了されることがある (MAY) ． Session は， Reauthentication イベント ( [Section 7.2](#sessionreauthn) で説明される ) を通して維持されてもよい (MAY) ．ユーザーは，初回の Authentication イベントの一部または全部を繰り返すことによってセッションを再確立する．

Session 管理は 頻繁な Credential の提示よりも望ましい． Credential を頻繁に提示することのユーザビリティの低さは，しばしばロック解除用 Credential のキャッシュなどの回避策に対するインセンティブを生み出し， Authentication イベントの新鮮さを無効にする．

### 7.1 Session Bindings
Session は， Subscriber が動作しているソフトウェア &mdash; ブラウザー，アプリケーション，オペレーティング システム (つまり Session Subject) &mdash; と， Subscriber がアクセスする RP や CSP (つまり Session ホスト) の間に発生する． Session シークレットは， Subscriber のソフトウェアとアクセスされているサービスの間で共有されることとする (SHALL) ． このシークレットは，Session の 2 つのエンドをバインドし， Subscriber が経時的にサービスを使用し続けることを許可する． シークレットは，Subscriber のソフトウェアによって直接提示されることとする (SHALL) ．または，シークレットの所有は暗号化メカニズムによって証明されることとする (SHALL) ．

Session のバインドに使用されるシークレットは， Session ホストによって Authentication イベントへの直接の応答の中で生成されることとする (SHALL) ． Session はその生成がトリガーされた Authentication イベントの AAL プロパティ 継承するべきである (SHOULD) ． Session は Authentication イベントよりも低い AAL で考えられてもよい (MAY) が， Authentication イベントよりも高い AAL では考えないこととする (SHALL NOT) ．

Session Binding に使用されるシークレットは :

1. インタラクションの間に，通常は Authentication の直後に Session ホストによって生成されることとする (SHALL) ．
2. 承認されたランダムビットジェネレーター [[SP 800-90Ar1]](#SP800-90Ar1) によって生成され，少なくとも64ビットの Entropy を含むこととする (SHALL) ．
3. Subscriber のログアウト時に， Session Subject によって消去または無効にされることとする (SHALL) ．
4. ユーザーがログアウトするとき，またはシークレットの期限が切れたとみなされるとき，Subscriber エンドポイントから消去されるべきである (SHOULD) ．
5. Cross-site Scripting (XSS) 攻撃によるローカルストレージ暴露の可能性があるため，HTML5 ローカル ストレージなどの安全でない場所に配置されるべきでない (SHOULD NOT) ．
6. Authenticated Protected Channel を使用してデバイスから送信，または受信されることとする (SHALL) ．
7. [Section 4.1.4](#aal1reauth)，[4.2.4](#aal2reauth)，[4.3.4](#aal3reauth) の AAL にふさわしい時間の後は，タイムアウトすることとし，受け付けられてはならないこととする (SHALL) ．
8. ホストと Subscriber のエンドポイント間の安全でないコミュニケーションでは有効にしないこととする (SHALL NOT) ． Authenticated Session は，Authentication の後，HTTPSからHTTPへというような安全でないトランスポートにフォールバックしないこととする (SHALL NOT) ．

URL か POST 内容は Session identifier を含むこととする (SHALL) ．Session identifier は， Session の外側で行われたアクションが Protected Session に影響しないことを保証することを RP によって検証されることとする (SHALL) ．

経時的に Session を管理するためにはいくつかのメカニズムがある． 次のセクションでは，各例のテクノロジー特有の追加要件と考慮事項に沿って，それぞれの例を示す． 追加の有用なガイダンスとして， OWASP *Session Management Cheat Sheet* [[OWASP-session]](#OWASP-session) がある．

#### 7.1.1. ブラウザークッキー
ブラウザークッキーは，サービスにアクセスする Subscriber のための Session の作成と追跡において，有力なメカニズムである．

クッキーは :

1. セキュアな (HTTPS) Session のみでアクセス可能なようにタグ付けされることとする (SHALL) ．
2. ホスト名とパスの最小で実際的なセットにアクセス可能であることとする (SHALL) ．
3. JavaScript (HttpOnly) からアクセス不可となるようにタグ付けされるべきである (SHOULD) ．
4. Sessionの有効な期間，またそのすぐ後に有効期限切れとなるようタグ付けされるべきである (SHOULD) ． この要件はクッキーの蓄積を制限することを目的としているが，強制的な Session タイムアウトへの依存はしないこととする (SHALL NOT) ．

#### 7.1.2 Access Tokens
Access Token — OAuth に見られるような — はアプリケーションが Authentication イベントの後，Subscriber のために一連のサービスに Access することを許可するために使用される． OAuth Access Token の存在は，他のシグナルがない場合，RP によって Subscriber の存在として解釈されないこととする (SHALL NOT) ． OAuth Access Token ，および関連付けられた Refresh Token は，Authentication Session が終了され，Subscriber そのアプリケーションから去ったあとでも，有効であってもよい (MAY) ．

#### 7.1.3 デバイスの識別
Subscriber とサービス間の Session の制定には，安全なデバイス識別の他の方法 &mdash; 相互 TLS や Token Binding ，その他のメカニズムを含み，これに限らない &mdash; が使用されてもよい (MAY) ．

### 7.2 Reauthentication
認証された Session の継続性は，Authentication の際に Verifier によって発行され，セッション中に任意に更新される Session シークレット の所有に基づくすることとする (SHALL) ． Session の性質は以下を含むアプリケーションに依存する :

1. Web ブラウザー Session と "Session" クッキー
2. Session シークレットを保持するモバイルアプリケーションのインスタンス

Session シークレットは非永続的であることとする (SHALL) ．つまり，関連するアプリケーションのリスタートやホストデバイスのリブートを超えて残置されないこととする (SHALL NOT) ．

Session の 定期的な Reauthentication は，認証された Session において，Subscriber が継続して存在していること (すなわち Subscriber がログアウトせずに立ち去っていないこと) を確認するために実行されることとする (SHALL) ．

Session は，Session シークレット単独の提示に基づいて，セクション [4.1.3](#aal1reauth), [4.2.3](#aal2reauth) そして [4.3.3](#aal3reauth) (AALに応じる) のガイドラインを超えて拡張されないこととする (SHALL NOT) ． Reauthentication のタイムリミットは，Session が 満了する前に，表 7-1で指定された Authentication Factor を Subscriber に促すことで延長されることとする (SHALL) ．

タイムアウトやその他のアクションによって Session が終了されたとき，ユーザーは再度の Authenticate によって新しい Session の確立を要求されることとする (SHALL) ．

**表 7-1 AAL Reauthentication 要件**

| AAL | 要件                                  |
| --- | ----------------------------------- |
| 1   | 任意の 1 つのファクターの提示                    |
| 2   | Memorized Secret または Biometrics の提示 |
| 3   | 全てのファクターの提示                         |

> ノート: AAL2では，Session シークレットは *something you have* であり，また追加の Authentication Factor が Session を継続するために要求されるため， 物理的な Authenticator でない，Memorized Secret または Biometrics が必須とされる．

#### 7.2.1 Federation または Assertion からの Reauthentication
CSP と RP を接続するために [SP 800-63C](sp800-63c.html) のセクション5で説明されている Federation プロトコルを使用するとき，Session 管理と Reauthentication は特別な考慮事項が適用される． Federation プロトコルは CSP と RP の間の Authentication イベントを執り行うが，それらの間に Session を確立しない． CSP と RP は，しばしば別々の Session 管理技術を採用しているので，これらのセッション間にはどんな相関も仮定しないこととする (SHALL NOT) ． したがって，RP の Session が満了し RP が再認証を必要とするとき，CSP の Session が満了しておらず，ユーザーの Reauthentication なしに CSP でこの Session から新しい Assertion が生成される可能性は十分にある．

<!-- When using a federation protocol as described in [SP 800-63C](sp800-63c.html), Section 5 to connect the CSP and RP, special considerations apply to session management and reauthentication. The federation protocol communicates an authentication event between the CSP and the RP but establishes no session between them. Since the CSP and RP often employ separate session management technologies, there SHALL NOT be any assumption of correlation between these sessions. Consequently, when an RP session expires and the RP requires reauthentication, it is entirely possible that the session at the CSP has not expired and that a new assertion could be generated from this session at the CSP without reauthenticating the user. -->

Federation プロトコルによる Reauthentication を必要とする RP は，— 可能な場合はプロトコル内で — CSPへの許容可能な最大の Authentication 経過時間を指定することとし (SHALL)，その期間内に Authentication が行われていない場合，CSP は Subscriber を Reauthentication することとする (SHALL) ． CSP は，RP が，Assertion が Reauthentication に十分かを決定でき且つ次の Reauthentication イベントの時間を決定するために， Authentication イベントの時間を RP へ伝えることとする (SHALL) ．

<!-- An RP requiring reauthentication through a federation protocol SHALL — if possible within the protocol — specify the maximum acceptable authentication age to the CSP, and the CSP SHALL reauthenticate the subscriber if they have not been authenticated within that time period. The CSP SHALL communicate the authentication event time to the RP to allow the RP to decide if the assertion is sufficient for reauthentication and to determine the time for the next reauthentication event. -->
<a name="sec8"></a>

## 8 脅威とセキュリティに関する考慮事項
<!-- ## 8 Threats and Security Considerations -->

*This section is informative.*

### 8.1 Authenticator の脅威
<!-- ### 8.1 Authenticator Threats -->

Authenticator を制御できる Attacker は Authenticator の所有者のように偽装することができる． Authenticator への脅威は，Authenticator を構成する Authentication 要素の種類ごとの Attack に基づいて分類されることができる :
<!-- An attacker who can gain control of an authenticator will often be able to masquerade as the authenticator's owner. Threats to authenticators can be categorized based on attacks on the types of authentication factors that comprise the authenticator: -->

* *Something you know* は攻撃者に開示されることがある． Attacker は Memorized Secret を推測するかもしれない． Authenticator が Shared Secret である場合，Attacker は CSP または Verifier への Access を得てシークレットの値を入手する，または，その値のハッシュに対して辞書 Attack を行うことができるだろう． Attacker は，PIN やパスコードの入力を観察したり，PIN やパスコードが書かれた記録やジャーナルエントリを見つけたり，またはシークレットをキャプチャーするために不正なソフトウェア (例：キーボードロガー) をインストールしたりするかもしれない． さらに，Attacker は Verifier によってメンテナンスされている Password データベースに対する Offline Attack によってシークレットを特定するかもしれない．
<!-- * *Something you know* may be disclosed to an attacker. The attacker might guess a memorized secret. Where the authenticator is a shared secret, the attacker could gain access to the CSP or verifier and obtain the secret value or perform a dictionary attack on a hash of that value. An attacker may observe the entry of a PIN or passcode, find a written record or journal entry of a PIN or passcode, or may install malicious software (e.g., a keyboard logger) to capture the secret. Additionally, an attacker may determine the secret through offline attacks on a password database maintained by the verifier. -->

* *Something you have* は紛失，破損，所有者から盗難される，または Attacker によって複製されることがある． たとえば，所有者のコンピューターへの Access を得た Attacker は，ソフトウェアの Authenticator をコピーするかもしれない． ハードウェアの Authenticator は，盗難されたり，いじられたり，複製されるかもしれない． Out-of-band のシークレットは，Attacker によって傍受され，彼らの Session を Authenticate するために利用されるかもしれない．
<!-- * *Something you have* may be lost, damaged, stolen from the owner, or cloned by an attacker. For example, an attacker who gains access to the owner's computer might copy a software authenticator. A hardware authenticator might be stolen, tampered with, or duplicated. Out-of-band secrets may be intercepted by an attacker and used to authenticate their own session. -->

* *Something you are* は複製されることがある．たとえば，Attacker は Subscriber の指紋のコピーを得たり，レプリカを製作するかもしれない．
<!-- * *Something you are* may be replicated. For example, an attacker may obtain a copy of the subscriber's fingerprint and construct a replica. -->

このドキュメントは Subscriber が Verifier に対して虚偽の Authenticate を試みようとしている Attacker と共謀してないことを前提としている． この前提に立って，Digital Authentication に使用される Authenticator への脅威はいくつかの例とともに [ 表 8-1](#63bSec8-Table1) にリストされる．
<!-- This document assumes that the subscriber is not colluding with an attacker who is attempting to falsely authenticate to the verifier. With this assumption in mind, the threats to the authenticator(s) used for digital authentication are listed in [Table 8-1](#63bSec8-Table1), along with some examples. -->

<a name="63bSec8-Table1"></a>

<div class="text-center" markdown="1">

**表 8-1 - Authenticator の脅威**
<!-- **Table 8-1  Authenticator Threats** -->

</div>


| **Authenticator の脅威/攻撃**  | **説明**  | **例**  |
| ------------------------- | --------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------- |
| **Assertion の製造または変更**  | Attacker が偽の Assertion を生成する  | 危殆化した CSP が 正しくAuthenticate されていない Claimant の Identity を主張する  |
| | Attacker が既存の Assertion を変更する  | Authentication Assertion の AAL を変更する危殆化したプロキシ  |
| **盗難**  | 物理的な Authenticator が Attacker によって盗難される  | ハードウェア暗号化デバイスが盗難される  |
| | | OTP デバイスが盗難される  |
| | | ルックアップシークレット Authenticator が盗難される  |
| | | 携帯電話が盗難される  |
| **複製**  | Subscriber の Authenticator が，本人の知識ごと，または知識なしに複製される  | Password が書かれた紙が開示される  |
|  |  | 電子ファイルに格納されている Password がコピーされる  |
|  |  | ソフトウェア PKI Authenticator (Private Key) がコピーされる  |
|  |  | ルックアップシークレット Authenticator がコピーされる  |
|  |  | 偽造された Biometric Authenticator が製造される  |
| **盗聴**  | Subscriber が認証を行っているときに， Authenticator Secret または Authenticator Output が攻撃者に暴露される  | キーボードエントリーを監視して Memorized secret を取得する  |
|  |  | キーストロークをロギングするソフトウェアによって，Memorized secret または Authenticator の出力を横取りする  |
|  |  | PIN パッドデバイスから PIN がキャプチャされる  |
|  |  | ハッシュされた Password が取得され，Attacker に別の Authentication で使用される (*pass-the-hash Attack*)  |
|  | Out of band シークレットが，通信チャネルの危殆化によって Attacker に傍受される  | Out of band シークレットが暗号化されていないWi-Fiで送信され，Attacker に受信される  |
| **オフライン クラッキング**  | Authenticator が，Authentication メカニズムの外側で解析メソッドによって明らかにされる  | Software PKI authenticator が，Private Key の復号に使用する正しい Password を識別するための辞書 Attack を受ける  |
| **サイドチャネル Attack**  | Authenticator Secret が， Authenticator の物理的特性を利用して明らかにされる．  | 鍵がハードウェア Cryptographic Authenticator の差分電力解析によって明らかにされる  |
|  |  | Authenticator への無数の試行の応答時間の解析によって， Cryptographic Authenticator シークレットが抽出される  |
| **Phishing または Pharming** | Attacker が Verifier や RP であると考えるように Subscriber をだますことで，Authenticator Output がキャプチャされる  | Subscriber が Verifier に偽装した Web サイトに入力することで，Password が明らかにされる  |
|  |  | 銀行 Subscriber が，銀行担当者に見せかけたフィッシャーからのメールに返信することで，Memorized Secret が明らかにされる  |
|  |  | Subscriber が DNS スプーフィングを介して偽の Verifier Webサイトにアクセスしてしまうことで，Memorized Secret が明らかにされる  |
| **Social Engineering**  | Attacker が，Subscriber 自身がAuthenticator Secret または Authenticator Output を明らかにするように説き伏せるために Subscriber と信頼関係を構築する | Subscriber の上司の代理として Password を聞いてきた同僚に対して Subscriber が Memorized Secret を明らかにする  |
|  |  | システム管理者を装った Attacker からの電話問い合わせによって，Subscriber が Memorized Secret を明らかにする  |
|  |  | Attacker が，犠牲者の携帯電話を Attacker にリダイレクトするようにモバイルオペレーターをやりこめることで，SMS経由の Out-of-band シークレットが Attacker に受信される |
| **オンラインでの推測**  | Attacker が Verifier にオンラインで接続し，その Verifier のコンテキストで有効な Authenticator Output を推測しようと試行する  | Memorized Secret を推測するためにオンライン辞書 Attack が利用される  |
|  |  | 正当な Claimant に登録された OTP デバイスの Authenticator Output を推測するために，オンラインでの推測が使用される  |
| **エンドポイントの危殆化**  | エンドポイント上の不正なコードが，接続された Authenticator への Remote Access を Subscriber 同意なしにプロキシする．  | エンドポイントに接続された Cryptographic Authenticator が，Remote から Attacker の Authenticate に使用される  |
|  | エンドポイント上の不正なコードが，Verifier が意図していない Authentication を引き起こす  | Authentication が，Subscriber ではなく Attacker のために行われる  |
|  |  | エンドポイント上の不正なアプリが SMS 経由で送信された Out-of-band シークレットを読みだし，Attacker がシークレットを Authenticate に使用する  |
|  | エンドポイント上の不正なコードが， Multi-Factor ソフトウェア Cryptographic Authenticator を危殆化させる  | 不正なコードが Authentication をプロキシする，またはエンドポイントから Authenticator の鍵をエクスポートする  |
| **認証されていない Binding**  | Attacker が彼らの管理下の Authenticator を Subscriber のアカウントにバインドさせる  | Attacker が Subscriber への経路の途中で Authenticator やプロビジョニングキーを傍受する  |

<!--
 | **Authenticator Threat/Attack**  | **Description**  | **Examples** |
|------------------------------------|------------------|--------------|
| **Assertion Manufacture or Modification** | The attacker generates a false assertion | Compromised CSP asserts identity of a claimant who has not properly authenticated |
| | The attacker modifies an existing assertion | Compromised proxy that changes AAL of an authentication assertion |
| **Theft** | A physical authenticator is stolen by an Attacker. | A hardware cryptographic device is stolen. |
| | | An OTP device is stolen. |
| | | A look-up secret authenticator is stolen. |
| | | A cell phone is stolen. |
| **Duplication** | The subscriber's authenticator has been copied with or without their knowledge. | Passwords written on paper are disclosed.
| | | Passwords stored in an electronic file are copied. |
| | | Software PKI authenticator (private key) copied. |
| | | Look-up secret authenticator copied. |
| | | Counterfeit biometric authenticator manufactured. |
| **Eavesdropping** | The authenticator secret or authenticator output is revealed to the attacker as the subscriber is authenticating. | Memorized secrets are obtained by watching keyboard entry. |
| | | Memorized secrets or authenticator outputs are intercepted by keystroke logging software. |
| | | A PIN is captured from a PIN pad device. |
| | | A hashed password is obtained and used by an attacker for another authentication (*pass-the-hash attack*). |
| | An out-of-band secret is intercepted by the attacker by compromising the communication channel. | An out-of-band secret is transmitted via unencrypted Wi-Fi and received by the attacker. |
| **Offline Cracking** | The authenticator is exposed using analytical methods outside the authentication mechanism. | A software PKI authenticator is subjected to dictionary attack to identify the correct password to use to decrypt the private key. |
| **Side Channel Attack** | The authenticator secret is exposed using physical characteristics of the authenticator. | A key is extracted by differential power analysis on a hardware cryptographic authenticator. |
| | | A cryptographic authenticator secret is extracted by analysis of the response time of the authenticator over a number of attempts. |
| **Phishing or Pharming** | The authenticator output is captured by fooling the subscriber into thinking the attacker is a verifier or RP. | A password is revealed by subscriber to a website impersonating the verifier.
| | | A memorized secret is revealed by a bank subscriber in response to an email inquiry from a phisher pretending to represent the bank. |
| | | A memorized secret is revealed by the subscriber at a bogus verifier website reached through DNS spoofing.
| **Social Engineering** | The attacker establishes a level of trust with a subscriber in order to convince the subscriber to reveal their authenticator secret or authenticator output. | A memorized secret is revealed by the subscriber to an officemate asking for the password on behalf of the subscriber's boss. |
| | | A memorized secret is revealed by a subscriber in a telephone inquiry from an attacker masquerading as a system administrator. |
| | | An out of band secret sent via SMS is received by an attacker who has convinced the mobile operator to redirect the victim's mobile phone to the attacker. |
| **Online Guessing** | The attacker connects to the verifier online and attempts to guess a valid authenticator output in the context of that verifier. | Online dictionary attacks are used to guess memorized secrets. |
| | | Online guessing is used to guess authenticator outputs for an OTP device registered to a legitimate claimant. |
| **Endpoint Compromise** | Malicious code on the endpoint proxies remote access to a connected authenticator without the subscriber's consent. | A cryptographic authenticator connected to the endpoint is used to authenticate remote attackers. |
| | Malicious code on the endpoint causes authentication to other than the intended verifier. | Authentication is performed on behalf of an attacker rather than the subscriber.
| | | A malicious app on the endpoint reads an out-of-band secret sent via SMS and the attacker uses the secret to authenticate.
| | Malicious code on the endpoint compromises a multi-factor software cryptographic authenticator. | Malicious code proxies authentication or exports authenticator keys from the endpoint.
| **Unauthorized Binding** | An attacker is able to cause an authenticator under their control to be bound to a subscriber's account. | An attacker intercepts an authenticator or provisioning key en route to the subscriber.
 -->

### 8.2 脅威を軽減するストラテジー
<!-- ### 8.2 Threat Mitigation Strategies -->
[表 8-2](#63bSec8-Table2) は，上記で説明された脅威の軽減を支援するメカニズムをまとめたものである．
<!-- Related mechanisms that assist in mitigating the threats identified above are summarized in [Table 8-2](#63bSec8-Table2). -->

<a name="63bSec8-Table2"></a>

<div class="text-center" markdown="1">

**表 8-2 - Authenticator の脅威を軽減する**
<!-- **Table 8-2 Mitigating Authenticator Threats** -->

</div>


| **Authenticator の脅威/攻撃**  | **脅威を軽減するメカニズム**  | **規約の参照**  |
| ------------------------- | ----------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------- |
| **盗難**  | Memorized Secret または Biometrics によってアクティブにされる必要がある Multi-Factor Authenticator を使用する | [4.2.1](#aal2types), [4.3.1](#aal3types)  |
|  | Memorized Secret または Biometrics を含む Authenticator を組み合わせて使用する  | [4.2.1](#aal2types), [4.3.1](#aal3types)  |
| **複製**  | Authentication シークレットの抽出や複製が長期的に困難な Authenticator を使用する  | [4.2.2](#aal2req), [4.3.2](#aal3req), [5.1.7.1](#sfcda)  |
| **盗聴**  | 特にキー ロガーなどのマルウェアに感染していないか，エンドポイントがセキュリティを確保できているか，使用する前に確かめる  | [4.2.2](#aal2req)  |
|  | 信頼されていないワイヤレスネットワークを，暗号化されていないセカンダリの out-of-band の Authentication チャネルとして使用することを避ける | [5.1.3.1](#ooba)  |
|  | 認証され，保護されたチャネル経由で Authenticate を行う (例 : ブラウザーウィンドウのロックアイコンを確認する)  | [4.1.2](#aal1req), [4.2.2](#aal2req), [4.3.2](#aal3req)  |
|  | *pass-the-hash* のようなリプレイ Attack に耐性のある Authentication プロトコルを使用する  | [5.2.8](#replay)  |
|  | 信頼された入力と信頼された表示機能の Authentication エンドポイントを使用する  | [5.1.6.1](#sfcsa), [5.1.8.1](#mfcsa)  |
| **オフライン クラッキング**  | 高い Entropy の Authenticator Secret で Authenticator を使用する  | [5.1.2.1](#lusa), [5.1.4.1](#sfotpa), [5.1.5.1](#mfotpa), [5.1.7.1](#sfcda), [5.1.9.1](#mfcda) |
|  | 鍵付きハッシュを含む，ソルト付き，ハッシュ化された状態で Memorized Secret を保存する  | [5.1.1.2](#memsecretver), [5.2.7](#verifier-secrets)  |
| **サイドチャネル Attack**  | シークレットの値に関係なく消費電力とタイミングが一定に保たれるように設計された Authenticator アルゴリズムを使用する  | [4.3.2](#aal3req)  |
| **Phishing または Pharming** | Verifier Impersonation への防御が提供される Authenticator を使用する．  | [5.2.5](#verifimpers)  |
| **Social Engineering**  | カスタマーサービスエージェントなどの第三者による Social Engineering のリスクを引き起こす Authenticator の使用を避ける  | [6.1.2.1](#bindexisting), [6.1.2.3](#replacement)  |
| **オンラインでの推測**  | 高い Entropy の出力を生成する Authenticator を使用する  | [5.1.2.1](#lusa), [5.1.7.1](#sfcda), [5.1.9.1](#mfcda)  |
|  | アクティベーションの試行に繰り返し失敗した後にはロックアップする Authenticator を使用する  | [5.2.2](#throttle)  |
| **エンドポイントの危殆化**  | Subscriber の物理的なアクションを必要とするハードウェア Authenticator を使用する  | [5.2.9](#intent)  |
|  | ソフトウェアベースの鍵は Restricted-Access ストレージで保持する  | [5.1.3.1](#ooba), [5.1.6.1](#sfcsa), [5.1.8.1](#mfcsa)  |
| **認証されていない Binding**  | Authenticator と 関連する鍵のプロビジョニングには MitM に体制のあるプロトコルを使用する  | [6.1](#binding)  |

<!--
| **Authenticator Threat/Attack** | **Threat Mitigation Mechanisms** | **Normative Reference(s)** |
|---------------------------------|----------------------------------|-------|
| **Theft** | Use multi-factor authenticators that need to be activated through a memorized secret or biometric.| [4.2.1](#aal2types), [4.3.1](#aal3types) |
| | Use a combination of authenticators that includes a memorized secret or biometric. | [4.2.1](#aal2types), [4.3.1](#aal3types) |
| **Duplication** |  Use authenticators from which it is difficult to extract and duplicate long-term authentication secrets. | [4.2.2](#aal2req), [4.3.2](#aal3req), [5.1.7.1](#sfcda) |
| **Eavesdropping** | Ensure the security of the endpoint, especially with respect to freedom from malware such as key loggers, prior to use. | [4.2.2](#aal2req) |
| | Avoid use of non-trusted wireless networks as unencrypted secondary out-of-band authentication channels. | [5.1.3.1](#ooba) |
| | Authenticate over authenticated protected channels (e.g., observe lock icon in browser window). | [4.1.2](#aal1req), [4.2.2](#aal2req), [4.3.2](#aal3req) |
| | Use authentication protocols that are resistant to replay attacks such as *pass-the-hash*. | [5.2.8](#replay) |
| | Use authentication endpoints that employ trusted input and trusted display capabilities. | [5.1.6.1](#sfcsa), [5.1.8.1](#mfcsa) |
| **Offline Cracking** | Use an authenticator with a high entropy authenticator secret. | [5.1.2.1](#lusa), [5.1.4.1](#sfotpa), [5.1.5.1](#mfotpa), [5.1.7.1](#sfcda), [5.1.9.1](#mfcda) |
| | Store memorized secrets in a salted, hashed form, including a keyed hash. | [5.1.1.2](#memsecretver), [5.2.7](#verifier-secrets) |
| **Side Channel Attack** | Use authenticator algorithms that are designed to maintain constant power consumption and timing regardless of secret values. | [4.3.2](#aal3req)
| **Phishing or Pharming** | Use authenticators that provide verifier impersonation resistance. | [5.2.5](#verifimpers) |
| **Social Engineering** | Avoid use of authenticators that present a risk of social engineering of third parties such as customer service agents. | [6.1.2.1](#bindexisting), [6.1.2.3](#replacement) |
| **Online Guessing** | Use authenticators that generate high entropy output. | [5.1.2.1](#lusa), [5.1.7.1](#sfcda), [5.1.9.1](#mfcda) |
| | Use an authenticator that locks up after a number of repeated failed activation attempts. | [5.2.2](#throttle) |
| **Endpoint Compromise** | Use hardware authenticators that require physical action by the subscriber. | [5.2.9](#intent) |
| | Maintain software-based keys in restricted-access storage. | [5.1.3.1](#ooba), [5.1.6.1](#sfcsa), [5.1.8.1](#mfcsa) |
| **Unauthorized Binding** | Use MitM-resistant protocols for provisioning of authenticators and associated keys. | [6.1](#binding) |
 -->

[表 8-1](#63bSec8-Table1) で説明された脅威を軽減するために，いくつかのストラテジーを適用できる :
<!-- Several other strategies may be applied to mitigate the threats described in [Table 8-1](#63bSec8-Table1): -->

* *複数の要素* があると，Attack がより成功しにくくなる． もし Attacker が Cryptographic Authenticator を盗むことと Memorized Secret の推定の両方を必要とするなら，その両方を達成することはとても困難になり得る．
<!-- * *Multiple factors* make successful attacks more difficult to accomplish. If an attacker needs to both steal a cryptographic authenticator and guess a memorized secret, then the work to discover both factors may be too high. -->

* *物理的なセキュリティメカニズム* は盗難された Authenticator を複製から保護するために採用されることがある．物理的なセキュリティメカニズムは，耐タンパー性の確保，検出，および対応を可能にする．
<!-- * *Physical security mechanisms* may be employed to protect a stolen authenticator from duplication. Physical security mechanisms can provide tamper evidence, detection, and response. -->

* 共有されている辞書に現れない *長い Memorized Secret の使用を要求する* ことは，Attacker にすべての入力可能な値を試させる．
<!-- * *Requiring the use of long memorized secrets* that don't appear in common dictionaries may force attackers to try every possible value. -->

* *システムと Network のセキュリティコントロール* は，Attacker がシステムへの Access を得ることや，不正なソフトウェアをインストールすることを防ぐために使用される．
<!-- * *System and network security controls* may be employed to prevent an attacker from gaining access to a system or installing malicious software. -->

* *定期的なトレーニング* は，いつどのように危殆化 —または危殆化の疑い— を報告するか Subscriber の理解を促したり，また，Attacker の Authentication プロセスを破るための試行を示す振る舞いのパターンを認識したりするために実施される．
<!-- * *Periodic training* may be performed to ensure subscribers understand when and how to report compromise — or suspicion of compromise — or otherwise recognize patterns of behavior that may signify an attacker attempting to compromise the authentication process. -->

* *Out of band テクニック* は，登録されたデバイス (例: 携帯電話) を所有していることの検証に使用される．
<!-- * *Out of band techniques* may be employed to verify proof of possession of registered devices (e.g., cell phones). -->

### 8.3 Authenticator の復旧
<!-- ### 8.3 Authenticator Recovery -->

多くの Authentication メカニズムの弱点は，Subscriber が 1 つまたは複数の Authenticator のコントロールを失い，それらを交換する必要がある場合の手続きである． 多くの場合，Subscriber を Authenticate できる残りの選択肢は限られている．経済的な心配 (例 : コールセンター運営のコスト) から，安価であまりセキュアでないバックアップの Authentication メソッドを使用したいと考えさせる． Authenticator の復旧は，人がアシストをするという点で，Social Engineering Attack のリスクもある．
<!-- The weak point in many authentication mechanisms is the process followed when a subscriber loses control of one or more authenticators and needs to replace them. In many cases, the options remaining available to authenticate the subscriber are limited, and economic concerns (e.g., cost of maintaining call centers) motivate the use of inexpensive, and often less secure, backup authentication methods. To the extent that authenticator recovery is human-assisted, there is also the risk of social engineering attacks. -->

Authentication 要素の完全性を維持するためには，1 つの要素を使用した Authentication を異なる要素の Authenticator を取得するために活用できないことが必要不可欠である． たとえば，Memorized Secret は新しいルックアップシークレットのリストを得るために使用できてはならない．
<!-- To maintain the integrity of the authentication factors, it is essential that it not be possible to leverage an authentication involving one factor to obtain an authenticator of a different factor. For example, a memorized secret must not be usable to obtain a new list of look-up secrets. -->

### 8.4 Session Attack
<!-- ### 8.4 Session Attacks -->

上記のディスカッションでは Authentication イベント自体に対する脅威に焦点を当てているが，Authentication イベントに続く Session 上のハイジャック Attack は同様のセキュリティインパクトを持つ可能性がある． [セクション 7](#sec7) の Session 管理ガイドラインは XSSのような Attack に対して Session の完全性を維持するために必要不可欠である． さらに，実行可能な内容が含まれていないことを確実にするために，[[OWASP-XSS-prevention]](#OWASP-XSS-prevention)に示されるすべての情報をサニタイズすることが重要である． これらのガイドラインでは，Session シークレットの漏洩に対してさらなる保護を提供するため，Session シークレットが Mobile Code にアクセスできないようにすることも推奨している．
<!-- The above discussion focuses on threats to the authentication event itself, but hijacking attacks on the session following an authentication event can have similar security impacts. The session management guidelines in [Section 7](#sec7) are essential to maintain session integrity against attacks, such as XSS. In addition, it is important to sanitize all information to be displayed [[OWASP-XSS-prevention]](#OWASP-XSS-prevention) to ensure that it does not contain executable content. These guidelines also recommend that session secrets be made inaccessible to mobile code in order to provide extra protection against exfiltration of session secrets. -->

もう一つの Authentication 後の脅威である Cross-site Request Forgery (CSRF) は，複数の Session を同時に有効にするユーザ傾向の利点を利用する． 有効なURLやリクエストが意図せずまたは悪意を持ってアクティブ化される可能性を防ぐために Web リクエストの中に Session 識別子を埋め込んで検証することが重要である．
<!-- Another post-authentication threat, cross-site request forgery (CSRF), takes advantage of users' tendency to have multiple sessions active at the same time. It is important to embed and verify a session identifier into web requests to prevent the ability for a valid URL or request to be unintentionally or maliciously activated. -->
<a name="sec9"></a>

## 9 プライバシーの考慮事項

<!-- ## 9 Privacy Considerations -->

*これらのプライバシーに関する考慮事項はセクション 4 のガイダンスを補足する．This section is informative.*

<!-- *These privacy considerations supplement the guidance in Section 4. This section is informative.* -->

### 9.1 プライバシー Risk Assessment

<!-- ### 9.1 Privacy Risk Assessment -->

[セクション 4.1.5](#aal1records), [4.2.5](#aal2records), [4.3.5](#aal3records) は CSP にレコード保有のためのプライバシー Risk Assessment を行わせることを必要とする．このようなプライバシー Risk Assessment が含まれる :

<!-- [Sections 4.1.5](#aal1records), [4.2.5](#aal2records), and [4.3.5](#aal3records) require the CSP to conduct a privacy risk assessment for records retention. Such a privacy risk assessment would include: -->

1. レコード保有が Subscriber に侵害や権限のない情報へのアクセスなどの問題を引き起こす可能性
2. そのような問題が発生した場合のインパクト

<!-- 1. The likelihood that the records retention could create a problem for the subscriber, such as invasiveness or unauthorized access to the information.
2. The impact if such a problem did occur. -->

CSP はリスク受容，リスク軽減，リスク共有など，特定されたプライバシーリスクをとった場合に起こることを合理的に正当化できるべきである． Subscriber の同意を得ることはリスク共有の形と見なされ，Subscriber が共有されたリスクを適切に評価し，受け入れる能力があることが合理的に期待される場合にのみ適している．

<!-- CSPs should be able to reasonably justify any response they take to identified privacy risks, including accepting the risk, mitigating the risk, and sharing the risk. The use of subscriber consent is a form of sharing the risk, and therefore appropriate for use only when a subscriber could reasonably be expected to have the capacity to assess and accept the shared risk. -->

### 9.2 プライバシー コントロール

<!-- ### 9.2 Privacy Controls -->

[セクション 4.4](#aal_privacy) は CSP が適切に調整されたプライバシーコントロールを採用することを必要とする． [SP 800-53](#SP800-53) は，CSP が Authentication メカニズムをデプロイするときに検討するべき一連のプライバシーコントロールを提供する． これらのコントロールは，成功し，信頼できるデプロイメントのための通知や救済策，その他の重要な考慮事項をカバーする．

<!-- [Section 4.4](#aal_privacy) requires CSPs to employ appropriately-tailored privacy controls. [SP 800-53](#SP800-53) provides a set of privacy controls for CSPs to consider when deploying authentication mechanisms. These controls cover notices, redress, and other important considerations for successful and trustworthy deployments. -->

### 9.3 使用制限

<!-- ### 9.3 Use Limitation -->

[セクション 4.4](#aal_privacy) は，Authentication 以外のいかなる目的，詐欺の緩和，または法律や法的手続きにおいて，CSP が明確な通知を提供し Subscriber からの追加の用途についての同意を取得しない限り，Authentication プロセス中に収集され保持される Authenticator についての情報の使用を禁止する． そのような情報の使用は，収集の本来の目的に限定されていることが保証されるよう注意が払われるべきである． 提案されたエージェンシーの使用がこのスコープに該当するが疑問がある場合は，あなたの SAOP に相談してほしい． [セクション 4.4](#aal_privacy) で述べられているように，Subscriber による追加の要件の受諾は，Authentication サービスを提供する条件としないこととする．

<!-- [Section 4.4](#aal_privacy) prohibits CSPs from using information about authenticators that is collected and maintained in the authentication process for any purpose other than authentication, related fraud mitigation, or to comply with law or legal process, unless the CSP provides clear notice and obtains consent from the subscriber for additional uses. Care should be taken to ensure that the use of such information is limited to its original purpose for collection. Consult your SAOP if there are questions about whether proposed agency uses fall within this scope. As stated in [Section 4.4](#aal_privacy), acceptance by the subscriber of additional uses SHALL NOT be a condition of providing authentication services. -->

### 9.4 <a name="agency-privacy"></a>Agency-Specific Privacy Compliance

<!-- ### 9.4 <a name="agency-privacy"></a>Agency-Specific Privacy Compliance -->

[セクション 4.4](#aal_privacy) は，連邦政府の CSP に関する特定の遵守義務をカバーする． プライバシーリスクを評価，軽減し，Authenticator を発行または維持する PII のコレクションが PIA を実施するための *Privacy Act of 1974* [Privacy Act](#PrivacyAct) や *E-Government Act of 2002* [E-Gov](#E-Gov) 要件をトリガーするかどうかなど，コンプライアンス要件について助言するように，Digital Authentication システム開発の最初期段階にあなたのエージェンシーの SAOP を参画させることが重要である． たとえば，Biometrics の集中保有に関して， Privacy Act requirements がトリガーされ，Authentication に必要なその他の属性や PII の収集，維持のために，新規または既存の Privacy Act 記録のシステムによってカバーされる必要があるかもしれない． SAOP は PIA が必要かどうかを判断する際， 同様にエージェンシーを支援することができる．

<!-- [Section 4.4](#aal_privacy) covers specific compliance obligations for federal CSPs. It is critical to involve your agency's SAOP in the earliest stages of digital authentication system development in order to assess and mitigate privacy risks and advise the agency on compliance requirements, such as whether or not the collection of PII to issue or maintain authenticators triggers the *Privacy Act of 1974* [Privacy Act](#PrivacyAct) or the *E-Government Act of 2002* [E-Gov](#E-Gov) requirement to conduct a PIA. For example, with respect to centralized maintenance of biometrics, it is likely that the Privacy Act requirements will be triggered and require coverage by either a new or existing Privacy Act system of records due to the collection and maintenance of PII and any other attributes necessary for authentication. The SAOP can similarly assist the agency in determining whether a PIA is required. -->

これらの考慮事項は Authentication だけのために Privacy Act SORN や PIA を開発するための要件として読まれるべきでない． 多くの場合，全体を網羅する Digital Authentication プロセスの PIA と SORN を下書きすることや，エージェンシーがオンラインで確立しているサービスやベネフィットを議論する，より大きなプログラマティック PIA の一部として Digital Authentication を含めることが最も理にかなっている．

<!-- These considerations should not be read as a requirement to develop a Privacy Act SORN or PIA for authentication alone. In many cases it will make the most sense to draft a PIA and SORN that encompasses the entire digital authentication process or include the digital authentication process as part of a larger programmatic PIA that discusses the service or benefit to which the agency is establishing online. -->

多くの Digital Authentication コンポーネントのために，SAOP がそれぞれ個別のコンポーネントの認識と理解を持つことが重要である． たとえば，他のプライバシー成果物は連携された CSP や RP のサービスを提供または使用するエージェンシーに適用可能であってもよい (例 : データ使用規約，コンピュータマッチング規約) ． SAOP は追加要件の適用を判断する際にエージェンシーを支援できる． また，Digital Authentication の個別のコンポーネントを完全に理解することは，コンプライアンスプロセスまたはほかの手段によって，SAOP が徹底的にプライバシーリスクを評価，軽減することを可能にする．

<!-- Due to the many components of digital authentication, it is important for the SAOP to have an awareness and understanding of each individual component. For example, other privacy artifacts may be applicable to an agency offering or using federated CSP or RP services (e.g., Data Use Agreements, Computer Matching Agreements). The SAOP can assist the agency in determining what additional requirements apply. Moreover, a thorough understanding of the individual components of digital authentication will enable the SAOP to thoroughly assess and mitigate privacy risks either through compliance processes or by other means. -->
<a name="sec10"></a>

<div class="breaker"></div>

## 10. ユーザビリティに関する考慮事項
_This section is informative._

[ISO/IEC 9241-11](#ISO9241) は，Usability を「特定のユーザが特定の使用状況において，有効性，効率，満足をもって特定のゴールを達成するためにある製品を使用できる程度」として定義している．この定義は有効性，効率，満足を達成する鍵となる要素として，ユーザ，そのゴール，使用状況に焦点を当てている． Usability を達成するためには，これらの鍵となる要素を占める全体論的なアプローチが必要である．

<!-- [ISO/IEC 9241-11](#ISO9241) defines usability as the "extent to which a product can be used by specified users to achieve specified goals with effectiveness, efficiency and satisfaction in a specified context of use." This definition focuses on users, their goals, and the context of use as key elements necessary for achieving effectiveness, efficiency, and satisfaction. A holistic approach that accounts for these key elements is necessary to achieve usability. -->


情報システムにアクセスするユーザのゴールは，意図したタスクを実行することである． Authentication はこのゴールを実現する機能である． しかしユーザの視点からは，Authentication はそれらと意図したタスクの間にある． Authentication の効果的な設計と実装は，正しいことを行うことを簡単にし，謝ったことを行うことを難しくし，誤ったことが起きたとき，回復することを簡単にする．

<!-- A user's goal for accessing an information system is to perform an intended task. Authentication is the function that enables this goal. However, from the user's perspective, authentication stands between them and their intended task. Effective design and implementation of authentication makes it easy to do the right thing, hard to do the wrong thing, and easy to recover when the wrong thing happens. -->

組織はステークホルダーの Digital Authentication エコシステム全体における全体的な影響を認識する必要がある． ユーザは多くの場合，一つか複数の Authenticator をそれぞれ異なる RP に対して採用する． 続いて，彼らはパスワードを覚えたり，どの Authenticator がどの RP のものかを思い出したり，複数の物理的な Authentication デバイスを持ち歩いたりすることに奮闘する． プアーな Usability はしばしば対抗メカニズムや意図しない回避策を生じさせ，結果としてセキュリティ管理の有効性を低下させるため，Authentication の Usability を評価することは極めて重要である．

<!-- Organizations need to be cognizant of the overall implications of their stakeholders' entire digital authentication ecosystem. Users often employ one or more authenticator, each for a different RP. They then struggle to remember passwords, to recall which authenticator goes with which RP, and to carry multiple physical authentication devices. Evaluating the usability of authentication is critical, as poor usability often results in coping mechanisms and unintended work-arounds that can ultimately degrade the effectiveness of security controls. -->

Usability を開発プロセスに取り入れることで，ユーザの Authentication ニーズ と組織のビジネス上のゴールに取り組みながら，セキュアで使いやすいAuthentication ソリューションをリードすることができる．

<!-- Integrating usability into the development process can lead to authentication solutions that are secure and usable while still addressing users' authentication needs and organizations' business goals. -->

デジタルシステムにおける Usability のインパクトは，適切な AAL を決定する際の Risk Assessment の一環として考慮される必要がある． より高い AAL の Authenticator はより良い Usability を提供することがあり，より低い AAL アプリケーションとしての使用を許すべきである．

<!-- The impact of usability across digital systems needs to be considered as part of the risk assessment when deciding on the appropriate AAL. Authenticators with a higher AAL sometimes offer better usability and should be allowed for use for lower AAL applications. -->

Authentication のための Federation を活用することで，多くの Usability の問題を楽にすることができるが，そのようなアプローチは [SP 800-63C](sp800-63c.html) で説明されているような独自のトレードオフを持つ．

<!-- Leveraging federation for authentication can alleviate many of the usability issues, though such an approach has its own tradeoffs, as discussed in [SP 800-63C](sp800-63c.html). -->

このセクションは一般的な Usability の考慮事項と実装方法を提供するが，特定のソリューションを推奨しない． 言及された実装は特定の Usability ニーズに取り組む革新的な技術的アプローチを促進するための例示である． さらに，Usability の考慮事項とその実装は one-size-fits-all のソリューションを予防する多くの要素に対して敏感である． たとえば，デスクトップコンピューティング環境で使っているフォントサイズは，小さな OTP デバイスのスクリーンではテキストを画面の外に出してしまうかもしれない． 選択した Authenticator の Usability 評価を実施することは，実装の極めて重要な要素である． ユーザの代表者，実際的なゴールとタスク，適切な使用状況とともに評価を行うことが重要である．

<!-- This section provides general usability considerations and possible implementations, but does not recommend specific solutions. The implementations mentioned are examples to encourage innovative technological approaches to address specific usability needs. Further, usability considerations and their implementations are sensitive to many factors that prevent a one-size-fits-all solution. For example, a font size that works in the desktop computing environment may force text to scroll off of a small OTP device screen. Performing a usability evaluation on the selected authenticator is a critical component of implementation. It is important to conduct evaluations with representative users, realistic goals and tasks, and appropriate contexts of use. -->

**前提**

<!-- **ASSUMPTIONS** -->

このセクションでは，「ユーザ」という単語は「Claimant」または「Subscriber」を意味する．

<!-- In this section, the term "users" means "claimants" or "subscribers." -->

ガイドラインと考慮事項はユーザ視点から記述される．

<!-- Guidelines and considerations are described from the users' perspective. -->

アクセシビリティは Usability とは異なり，このドキュメントの対象外である． [セクション 508](#Section508) は情報技術のバリアを排除し，連邦政府機関に対して障害を持つ人々がオンラインの公開コンテンツにアクセスできるようにすることを求めるために制定された． アクセシビリティのガイダンスについてはセクション 508 の法律と標準を参照すること．

<!-- Accessibility differs from usability and is out of scope for this document. [Section 508](#Section508) was enacted to eliminate barriers in information technology and require federal agencies to make their online public content accessible to people with disabilities. Refer to Section 508 law and standards for accessibility guidance. -->

### <a name="usabilitycommon"></a> 10.1 Authenticator に共通する Usability の考慮事項

<!-- ### <a name="usabilitycommon"></a> 10.1 Usability Considerations Common to Authenticators -->

Authentication システムの選択や実装を行う際は，ユーザ，そのゴール，使用状況の組み合わせに注意しながら，選択した Authenticator のライフサイクル全体にわたる Usability (例 : 代表的な使用法や断続的なイベント) を検討する．

<!-- When selecting and implementing an authentication system, consider usability across the entire lifecycle of the selected authenticators (e.g., typical use and intermittent events), while being mindful of the combination of users, their goals, and context of use. -->

単一の Authenticator Type では通常，利用人口のすべてを満足させることはできない． したがって，可能な限り — AAL の要件に基づいて — CSPは代わりの Authenticator をサポートし，ユーザが必要に応じて選択できるようにするべきである． 多くの場合，タスクの即時性，知覚されるコストベネフィットのトレードオフ，特定の Authenticator に対する不慣れさが選択に影響を与える． ユーザはその時点での最小の負担またはコストとなる選択肢を選ぶ傾向がある． たとえば，タスクが情報システムへの即時の Access を必要とするとき，ユーザはより多くのステップを必要とする Authenticator を選択するのではなく，新しいアカウントと Password を作成することを好むかもしれない． あるいは，ユーザがすでに Identity プロバイダのアカウントを持っている場合，(適切なAALで承認された) Identity 連携という選択肢を選ぶかもしれない． ユーザはいくつかの Authenticator を他のものよりもよく理解し，また彼らの理解と経験に基づいた異なる信頼レベルを持っている．

<!-- A single authenticator type usually does not suffice for the entire user population. Therefore, whenever possible — based on AAL requirements — CSPs should support alternative authenticator types and allow users to choose based on their needs. Task immediacy, perceived cost benefit tradeoffs, and unfamiliarity with certain authenticators often impact choice. Users tend to choose options that incur the least burden or cost at that moment. For example, if a task requires immediate access to an information system, a user may prefer to create a new account and password rather than select an authenticator requiring more steps. Alternatively, users may choose a federated identity option — approved at the appropriate AAL — if they already have an account with an identity provider. Users may understand some authenticators better than others, and have different levels of trust based on their understanding and experience. -->

ポジティブな Authentication の経験は，望ましいビジネス成果を達成する組織の成功に不可欠である． したがって，ユーザ視点から Authenticator を検討するべきである． 包括的な Authentication Usability のゴールは，ユーザの負担と Authentication の摩擦 (例 : ユーザが Authenticate する回数，関連するステップ，ユーザが追跡しなければならない情報の量) を最小限に抑えることである． シングルサインオンはそのような最小化ストラテジーの一つを例示する．

<!-- Positive user authentication experiences are integral to the success of an organization achieving desired business outcomes. Therefore, they should strive to consider authenticators from the users' perspective. The overarching authentication usability goal is to minimize user burden and authentication friction (e.g., the number of times a user has to authenticate, the steps involved, and the amount of information he or she has to track). Single sign-on exemplifies one such minimization strategy. -->

ほとんどの Authenticator に適用可能な Usability の考慮事項を以下に説明する．後続のセクションでは，特定の Authenticator に固有の Usability の考慮事項について説明する．

<!-- Usability considerations applicable to most authenticators are described below. Subsequent sections describe usability considerations specific to a particular authenticator. -->

すべての Authenticator の代表的な使用法に関する Usability の考慮事項 :

<!-- Usability considerations for typical usage of all authenticators include: -->

* Authenticator の使用と保守に関連する情報を提供する．たとえば Authenticator の紛失や盗難があった場合の対応方法や使用方法 (特に初回の使用や初期化の際に異なる要件がある場合) ．

<!-- * Provide information on the use and maintenance of the authenticator, e.g., what to do if the authenticator is lost or stolen, and instructions for use — especially if there are different requirements for first-time use or initialization. -->

* ユーザが自分の Authenticator をすぐに利用できるようにするため，Authenticator の可用性も考慮されるべきである ． 紛失，破損，その他の元の Authenticator へのネガティブなインパクトに対応するため，代替の Authentication の選択肢が必要であると考える．

<!-- * Authenticator availability should also be considered as users will need to remember to have their authenticator readily available. Consider the need for alternate authentication options to protect against loss, damage, or other negative impacts to the original authenticator. -->

* 可能な限り，AAL の要件に基づいて，ユーザは代替の Authentication の選択肢を提供されるべきである． これによりユーザは，コンテキスト，ゴール，およびタスク (例 : タスクの頻度や即時性) に基づいて Authenticator を選択することができる． 代替の Authentication の選択肢は特定の Authenticator で発生する可用性の問題にも役立つ．

<!-- * Whenever possible, based on AAL requirements, users should be provided with alternate authentication options. This allows users to choose an authenticator based on their context, goals, and tasks (e.g., the frequency and immediacy of the task). Alternate authentication options also help address availability issues that may occur with a particular authenticator. -->

* ユーザ向けテキストの特徴 :
  * 意図されたオーディエンスに対して，平易な言語でユーザ向けテキストを書く (例 : 説明，指示，通知，エラーメッセージなど) ． 専門的な技術用語を避け，通常，第6学年から第8学年の識字レベルで書く．
  * ユーザ向けおよびユーザが入力したテキストについて，フォントのスタイル，サイズ，色，周囲の背景とのコントラストなどを含む可読性を考慮する． 読みづらいテキストはユーザ入力のミスを引き起こす． 可読性を高めるため，以下の使用を検討する :
    * ハイコントラスト．最も高いコントラストは黒上の白である．
    * 電子ディスプレイ用には Sans serif フォント． 印刷用には Serif フォント．
    * 混乱しやすい文字を明確に区別するフォント (例 : 大文字の「O」と数字「0」など ) ．
    * デバイスの画面に収まる限りは，最小フォントサイズを 12 ポイントとする．

<!-- * Characteristics of user-facing text:
  * Write user-facing text (e.g., instructions, prompts, notifications, error messages) in plain language for the intended audience. Avoid technical jargon and, typically, write for a 6th to 8th grade literacy level.
  * Consider the legibility of user-facing and user-entered text, including font style, size, color, and contrast with surrounding background. Illegible text contributes to user entry errors. To enhance legibility, consider the use of:
    * High contrast. The highest contrast is black on white.
    * Sans serif fonts for electronic displays. Serif fonts for printed materials.
    * Fonts that clearly distinguish between easily confusable characters (e.g., the capital letter "O" and the number "0").
    * A minimum font size of 12 points as long as the text fits for display on the device. -->

* Authenticator 入力中のユーザ体験 :
  * マスクされたテキスト入力はエラーを起こしやすいので，入力中にテキストを表示するオプションを提供する． ユーザに対して一度十分に表示された文字はまた非表示にされる． 従来のデスクトップコンピュータよりもモバイルデバイス (例 : タブレットやスマートフォン ) では Memorized Secret を入力するのに時間がかかるため，マスキング遅延時間を決定する際にはデバイスを考慮する． マスキング遅延時間がユーザのニーズと一致していることを確認する．
  * テキスト入力に不足しない時間が与えられていることを確認する (すなわち，入力画面が尚早にタイムアウトしない ) ．与えられたテキスト入力時間がユーザのニーズと一致していることを確認する．
  * ユーザの混乱や不満を軽減するため，入力エラーに対する明確，有意義かつアクション可能なフィードバックを提供する． ユーザが誤ってテキストを入力したことを知らない場合，重大な Usability への影響が生じる．
  * ユーザが Authenticator Output を入力する必要がある Authenticator に対して，少なくとも10回の入力を許す． 入力テキストがより長く複雑になるほど，ユーザ入力エラーの可能性は大きくなる．
  * 残された試行回数について，明確で有意義なフィードバックを提供する． 混乱と不満を軽減するため，レート制限 (すなわち，スロットリング) によって次の試行までどらくらい待たなければならないかを知らせる．

<!-- * User experience during authenticator entry:
  * Offer the option to display text during entry, as masked text entry is error-prone. Once a given character is displayed long enough for the user to see, it can be hidden. Consider the device when determining masking delay time, as it takes longer to enter memorized secrets on mobile devices (e.g., tablets and smartphones) than on traditional desktop computers. Ensure masking delay durations are consistent with user needs.
  * Ensure the time allowed for text entry is adequate (i.e., the entry screen does not time out prematurely). Ensure allowed text entry times are consistent with user needs.
  * Provide clear, meaningful and actionable feedback on entry errors to reduce user confusion and frustration. Significant usability implications arise when users do not know they have entered text incorrectly.
  * Allow at least 10 entry attempts for authenticators requiring the entry of the authenticator output by the user. The longer and more complex the entry text, the greater the likelihood of user entry errors.
  * Provide clear, meaningful feedback on the number of remaining allowed attempts. For rate limiting (i.e., throttling), inform users how long they have to wait until the next attempt to reduce confusion and frustration. -->

* モバイルデバイスでのタッチ領域や表示領域の制限など，フォーム要因の制約の影響を最小限に抑える :
  * 小さなデバイスでのタイピングはフルサイズキーボードでのタイピングよりもはるかにエラーが発生しやすく時間がかかるため，大きなタッチ領域は Usability を向上させる． 画面上のキーボードサイズが小さいほど，画面上のターゲットのサイズに対する入力メカニズム (例 : 指など) のサイズにより，タイプすることがより困難になる．
  * 小さなディスプレイのための優良なユーザインターフェースと情報デザインを追及する．

<!-- * Minimize the impact of form-factor constraints, such as limited touch and display areas on mobile devices:
  * Larger touch areas improve usability for text entry since typing on small devices is significantly more error prone and time consuming than typing on a full-size keyboard. The smaller the onscreen keyboard, the more difficult it is to type, due to the size of the input mechanism (e.g., a finger) relative to the size of the on-screen target.
  * Follow good user interface and information design for small displays. -->

断続的なイベントには，Reauthentication，アカウントロックアウト，有効期限切れ，失効，破損，紛失，盗難，機能しないソフトウェアなどのイベントが含まれる．

<!-- Intermittent events include events such as reauthentication, account lock-out, expiration, revocation, damage, loss, theft, and non-functional software. -->

Authenticator タイプ間の断続的なイベントに関する Usability の考慮事項 :

<!-- Usability considerations for intermittent events across authenticator types include: -->

* ユーザの非アクティブ状態によって Reauthenticate が必要となることを予防するために，非アクティブタイムアウトが起こらないようその直前(例 : 2分など) に活動をトリガーするためのプロンプトを出す．

<!-- * To prevent users from needing to reauthenticate due to user inactivity, prompt users in order to trigger activity just before (e.g., 2 minutes) an inactivity timeout would otherwise occur. -->

* ユーザの操作に関係のない定期的な Reauthentication イベントが必要となる前には，ユーザが作業を保存することを十分な時間 ( 例 : 1時間など ) を以って促す．

<!-- * Prompt users with adequate time (e.g., 1 hour) to save their work before the fixed periodic reauthentication event required regardless of user activity. -->

* どのようにして，どこで技術的な支援を得られるか明確にする． たとえば，オンラインセルフサービス機能へのリンク，チャットセッション，ヘルプデスクサポートのための電話番号などの情報をユーザに提供する． 理想的には，十分な情報が提供された場合は，外部からの介入なしにユーザが断続的なイベントから自分たちで回復できるようになる．

<!-- * Clearly communicate how and where to acquire technical assistance. For example, provide users with information such as a link to an online self-service feature, chat sessions or a phone number for help desk support. Ideally, sufficient information can be provided to enable users to recover from intermittent events on their own without outside intervention. -->

### 10.2 Authenticator Type による Usability の考慮事項

<!-- ### 10.2 Usability Considerations by Authenticator Type -->

ほとんどの Authenticator ( [セクション 10.1](#usabilitycommon) ) に適用可能な前述の一般的な Usability の考慮事項に加えて，以下のセクションでは，特定の Authenticator Type に固有のその他の Usability の考慮事項について説明する．

<!-- In addition to the previously described general usability considerations applicable to most authenticators ([Section 10.1](#usabilitycommon)), the following sections describe other usability considerations specific to particular authenticator types. -->

#### <a name="memorizedsecrets"></a> 10.2.1 Memorized Secret

<!-- #### <a name="memorizedsecrets"></a> 10.2.1 Memorized Secrets -->

**_代表的な使用法_**

<!-- **_Typical Usage_** -->

ユーザは，Memorized Secret (一般的に Password や PIN といわれる) を手動で入力する．

<!-- Users manually input the memorized secret (commonly referred to as a password or PIN). -->

典型的な使用法のための Usability の考慮事項 :

<!-- Usability considerations for typical usage include: -->

* Memorized Secret の記憶しやすさ
  * ユーザが覚えておくべき項目が多いほど，思い出すことに失敗する可能性は大きくなる． Memorized Secret が少なくなるほど，特定の RP に必要な特定の Memorized Secret をより簡単に思い出すことができる．
  * 使用頻度の低い Password は記憶負担が大きくなる．

<!-- * Memorability of the memorized secret.
  * The likelihood of recall failure increases as there are more items for users to remember. With fewer memorized secrets, users can more easily recall the specific memorized secret needed for a particular RP.
  * The memory burden is greater for a less frequently used password. -->

* Memorized Secret 入力中のユーザ体験  
  * Passphrase を含む Memorized Secret を入力するためのフィールドにコピーアンドペースト機能をサポートする．

<!-- * User experience during entry of the memorized secret.
  * Support copy and paste functionality in fields for entering memorized secrets, including passphrases. -->

**_断続的なイベント_**

<!-- **_Intermittent Events_** -->

断続的なイベントの Usability の考慮事項は以下を含む :

<!-- Usability considerations for intermittent events include: -->

* ユーザが Memorized Secret を作成または変更するとき :
  * Memorized Secret の作成や変更の方法を情報を明確に伝える．
  * [セクション5.1.1](#reqauthtype) で指定されているように，Memorized Secret の要件を明確に伝える．
  * Passphrase の使用をサポートするには，少なくとも64文字の長さを許容する． ユーザが空白を含む任意の文字を使用して，望み通りの長さで Memorized Secret を作成し記憶を手助けすることを奨励する．
  * Memorized Secret に他の構成ルール ( 例 : 異なる文字タイプの混合) を課さない．
  * ユーザの要求や Authenticator の危殆化の証拠がない限り，Memorized Secret は恣意的な (例 : 定期的な) 変更を必要としない． ( 詳細については [セクション 5.1.1](#reqauthtype) を参照 )

<!-- * When users create and change memorized secrets:
  * Clearly communicate information on how to create and change memorized secrets.
  * Clearly communicate memorized secret requirements, as specified in [Section 5.1.1](#reqauthtype).
  * Allow at least 64 characters in length to support the use of passphrases. Encourage users to make memorized secrets as lengthy as they want, using any characters they like (including spaces), thus aiding memorization.
  * Do not impose other composition rules (e.g. mixtures of different character types) on memorized secrets.
  * Do not require that memorized secrets be changed arbitrarily (e.g., periodically) unless there is a user request or evidence of authenticator compromise. (See [Section 5.1.1](#reqauthtype) for additional information). -->

* 選択された Password を拒否された場合，( 例 : 受け入れられない Password の"ブラックリスト"に含まれる，過去に使用されたなど ) 明確，有意義でアクション可能なフィードバックを提供する．

<!-- * Provide clear, meaningful and actionable feedback when chosen passwords are rejected (e.g., when it appears on a "black list" of unacceptable passwords or has been used previously). -->


#### 10.2.2 ルックアップ シークレット

**_代表的な使用法_**

<!-- **_Typical Usage_** -->

ユーザは印刷物または電子的な Authenticator を使用して，Verifier の問いかけに応答するために必要な適切なシークレットを探す． たとえば，ユーザはカードにテーブル形式で印刷された数字または文字列のうち，特定のサブセットを提供するように求められる．

<!-- Users use the authenticator — printed or electronic — to look up the appropriate secret(s) needed to respond to a verifier's prompt. For example, a user may be asked to provide a specific subset of the numeric or character strings printed on a card in table format. -->

代表的な使用法のための Usability の考慮事項 :

<!-- Usability considerations for typical usage include: -->

* ルックアップシークレット入力中のユーザ体験
  * プロンプトの複雑さとサイズを考慮する． ユーザが探すように促されるシークレットのサブセットが大きいほど，Usability への影響は大きくなる． Authentication のためのルックアップシークレットの量と複雑さを選択する際には，認知的作業負荷と入力の物理的な難しさの両方を考慮に入れるべきである．

<!-- * User experience during entry of look-up secrets.
  * Consider the prompts' complexity and size. The larger the subset of secrets a user is prompted to look up, the greater the usability implications. Both the cognitive workload and physical difficulty for entry should be taken into account when selecting the quantity and complexity of look-up secrets for authentication. -->

#### 10.2.3 Out-of-Band

<!-- #### 10.2.3 Out-of-Band -->

**_代表的な使用法_**

<!-- **_Typical Usage_** -->

Out-of-band authentication では，ユーザはプライマリとセカンダリの通信チャネルへの Access を持つ必要がある．

<!-- Out-of-band authentication requires users have access to a primary and secondary communication channel. -->

代表的な使用法のための Usability の考慮事項 :

<!-- Usability considerations for typical usage: -->

* ロックされたデバイス上でシークレットの受信を通知する．ただし Out-of-Band デバイスがロックされている場合，シークレットに Access するにはデバイスへの Authentication が必要とされるべきである．

<!-- * Notify users of the receipt of a secret on a locked device. However, if the out of band device is locked, authentication to the device should be required to access the secret. -->

* 実装に応じて，ユーザがモバイルデバイスにテキストを入力しなければならない場合に特に問題となるフォーム要因の制約を考慮する． より大きなタッチ領域を提供することはモバイルデバイスでシークレットを入力する際の Usability を向上させる．

<!-- * Depending on the implementation, consider form-factor constraints as they are particularly problematic when users must enter text on mobile devices. Providing larger touch areas will improve usability for entering secrets on mobile devices. -->

* より良い Usability の選択肢は，モバイルデバイス上にテキスト入力を必要としない機能を提供することだ ( 例 : ユーザが Out-of-Band シークレットをコピーアンドペースト出来るような画面上でのシングルタップ，コピー機能など ) ． このような機能をユーザに提供することは，プライマリチャネルとセカンダリチャネルが同じデバイス上にある場合に特に役立つ． たとえば，ユーザがスマートフォンで Authentication Secret を転送することは，Out-of-Band アプリケーションとプライマリチャネルを ( もしかすると複数回にわたり ) 前後に切り替える必要があるため，困難である．

<!-- * A better usability option is to offer features that do not require text entry on mobile devices (e.g., a single tap on the screen, or a copy feature so users can copy and paste out-of-band secrets). Providing users such features is particularly helpful when the primary and secondary channels are on the same device. For example, it is difficult for users to transfer the authentication secret on a smartphone because they must switch back and forth—potentially multiple times—between the out of band application and the primary channel. -->

#### 10.2.4 Single-Factor OTP デバイス

<!-- #### 10.2.4 Single-Factor OTP Device -->

**_代表的な使用法_**

<!-- **_Typical Usage_** -->

ユーザは Single-Factor OTP デバイスによって生成された OTP に Access する．Authenticator Output は通常デバイスに表示され，ユーザは Verifier のためにそれを入力する．

<!-- Users access the OTP generated by the single-factor OTP device. The authenticator output is typically displayed on the device and the user enters it for the verifier. -->

代表的な使用法のための Usability の考慮事項は以下を含む :

<!-- Usability considerations for typical usage include: -->

* Authenticator Output は少なくとも変更までに1分を許容する．しかし理想的には [セクション 5.1.4.1](#sfotpa) で指定されているように満2分間を許容する． ユーザは Authenticator Output を入力するのに十分な時間を必要とする ( Single-Factor OTP デバイスとその入力画面の間を前後に行き来することを含む ) ．

<!-- * Authenticator output allows at least one minute between changes, but ideally allows users the full two minutes as specified in [Section 5.1.4.1](#sfotpa). Users need adequate time to enter the authenticator output (including looking back and forth between the single-factor OTP device and the entry screen). -->

* 実装に応じた，実装者にとっての追加の Usability の考慮事項は以下のとおり :
  * もし Single-Factor OTP デバイスがそのアウトプットを電子インターフェース ( 例 : USBなど ) を介して提供する場合，ユーザが Authenticator Output を手動で入力する必要がないことが望ましい． しかし，物理的インプットの操作 ( 例 : ボタンを押すなど ) が必要とされる場合，USBポートの位置は Usability の問題となる可能性がある． たとえば，一部のコンピュータの USB ポートはコンピュータの背面にあり，ユーザの手が届きにくくなる．
  * USB ポートのような直接的コンピュータインターフェースの利用が限られる場合は Usability の問題を引き起こすことがある． たとえば，ラップトップコンピュータの USB ポートの数はしばしばとても限られる． これはユーザに対して Single-Factor OTP デバイスのために他の USB 周辺機器の接続解除を強制するかもしれない．

<!-- * Depending on the implementation, the following are additional usability considerations for implementers:
  * If the single-factor OTP device supplies its output via an electronic interface (e.g, USB) this is preferable since users do not have to manually enter the authenticator output. However, if a physical input (e.g., pressing a button) is required to operate, the location of the USB ports could pose usability difficulties. For example, the USB ports of some computers are located on the back of the computer and will be difficult for users to reach.
  * Limited availability of a direct computer interface such as a USB port could pose usability difficulties. For example, the number of USB ports on laptop computers is often very limited. This may force users to unplug other USB peripherals in order to use the single-factor OTP device. -->

#### 10.2.5 Multi-Factor OTP デバイス

<!-- #### 10.2.5 Multi-Factor OTP Device -->

**_代表的な使用法_**

<!-- **_Typical Usage_** -->

ユーザは第二の Authentication Factor を通じて Multi-Factor OTP デバイスによって生成された OTP に Access する． 通常 OTP はデバイスに表示され，Verifier のためにそれを手動で入力する． 第二の Authentication Factor は Memorized Secret ，一体型 Biometrics ( 例 : 指紋など ) リーダー，または直接的なコンピュータインターフェース ( 例 : USB ポート ) などの統合的なエントリーパッドに入力することで達成されてもよい． 追加の要素に対する Usability の考慮事項も同様に適用される． — Multi-Factor Authenticator に使用される Memorized Secret については [セクション 10.2.1](#memorizedsecrets)，Biometrics については [Section 10.4](#biomusability) を参照．

<!-- Users access the OTP generated by the multi-factor OTP device through a second authentication factor. The OTP is typically displayed on the device and the user manually enters it for the verifier. The second authentication factor may be achieved through some kind of integral entry pad to enter a memorized secret, an integral biometric (e.g., fingerprint) reader, or a direct computer interface (e.g., USB port). Usability considerations for the additional factor apply as well — see [Section 10.2.1](#memorizedsecrets) for memorized secrets and [Section 10.4](#biomusability) for biometrics used in multi-factor authenticators. -->

代表的な使用法のための Usability の考慮事項は以下を含む :

<!-- Usability considerations for typical usage include: -->

* Authenticator Output の手動入力中のユーザ体験
  * タイムベース OTP の場合は，OTP が表示されている時間に加えて猶予期間を設ける． ユーザは Multi-Factor OTP デバイスとその入力画面の間を前後に行き来することを含む，Authenticator Output を入力するのに十分な時間を必要とする．
  * ユーザが統合的なエントリーパッドを使用するか，モバイルデバイス上に Authenticator Output を入力して Multi-Factor OTP デバイスのロックを解除する必要がある場合は，フォーム要因の制約について考慮する． 小さなデバイスでのタイピングは従来のキーボードよりはるかにエラーが発生しやすく，時間がかかる． 統合的なエントリーパッドとオンスクリーンキーボードが小さくなるほど，タイプすることがより難しくなる． より大きなタッチ領域を提供することは Multi-Factor OTP デバイスのロックを解除する，またはモバイルデバイスで Authenticator Output を入力する際の Usability を向上させる．
  * USB ポートのような直接的コンピュータインターフェースの利用が限られる場合は Usability の問題を引き起こすことがある． たとえば，ラップトップコンピュータの USB ポートの数はしばしばとても限られるため，ユーザに対して Multi-Factor OTP デバイスのために他の USB 周辺機器の接続解除を強制するかもしれない．

<!-- * User experience during manual entry of the authenticator output.
  * For time-based OTP, provide a grace period in addition to the time during which the OTP is displayed. Users need adequate time to enter the authenticator output, including looking back and forth between the multi-factor OTP device and the entry screen.
  * Consider form-factor constraints if users must unlock the multi-factor OTP device via an integral entry pad or enter the authenticator output on mobile devices. Typing on small devices is significantly more error prone and time-consuming than typing on a traditional keyboard. The smaller the integral entry pad and onscreen keyboard, the more difficult it is to type. Providing larger touch areas improves usability for unlocking the multi-factor OTP device or entering the authenticator output on mobile devices.
  * Limited availability of a direct computer interface like a USB port could pose usability difficulties. For example, laptop computers often have a limited number of USB ports, which may force users to unplug other USB peripherals to use the multi-factor OTP device. -->

#### 10.2.6 Single-Factor 暗号ソフトウェア

<!-- #### 10.2.6 Single-Factor Cryptographic Software -->

**_代表的な使用法_**

<!-- **_Typical Usage_** -->

ユーザは暗号ソフトウェアキーの所持とコントロールを証明することによって Authenticate する．

<!-- Users authenticate by proving possession and control of the cryptographic software key. -->

代表的な使用法のための Usability の考慮事項は以下を含む :

<!-- Usability considerations for typical usage include: -->

* ユーザがどの Authentication タスクに使用する Cryptographic Key であるかを認識して呼び出す必要があるため，Cryptographic Key にはユーザにとって意味のある適切で説明的な名前がつけられる． これはユーザが，似ている，またはあいまいな名前の複数の Cryptographic Key を扱うことを予防する． より小さなモバイルデバイス上の複数の Cryptographic Key から選択することは，画面サイズが小さくなるために Cryptographic Key の名前が短縮されると特に問題になる．

<!-- * Give cryptographic keys appropriately descriptive names that are meaningful to users since users have to recognize and recall which cryptographic key to use for which authentication task. This prevents users from having to deal with multiple similarly- and ambiguously-named cryptographic keys. Selecting from multiple cryptographic keys on smaller mobile devices may be particularly problematic if the names of the cryptographic keys are shortened due to reduced screen size. -->

#### 10.2.7 Single-Factor 暗号デバイス

<!-- #### 10.2.7 Single-Factor Cryptographic Device -->

**_代表的な使用法_**

<!-- **_Typical Usage_** -->

ユーザは Single-Factor 暗号デバイスの所持を証明することで Authenticate する．

<!-- Users authenticate by proving possession of the single-factor cryptographic device. -->

代表的な使用法のための Usability の考慮事項は以下を含む :

<!-- Usability considerations for typical usage include: -->

* Single-Factor 暗号デバイスの操作として物理的インプット ( 例 : ボタンを押すなど ) を必要とする場合， Usability の問題となる可能性がある． たとえば，いくつかの USB ポートはコンピュータの背面にあり，ユーザの手が届きにくい．

<!-- * Requiring a physical input (e.g., pressing a button) to operate the single-factor cryptographic device could pose usability difficulties. For example, some USB ports are located on the back of computers, making it difficult for users to reach. -->

* USB ポートのような直接的コンピュータインターフェースの利用が限られる場合は Usability の問題を引き起こすことがある． たとえば，ラップトップコンピュータの USB ポートの数はしばしばとても限られるため，ユーザに対して Single-Factor 暗号デバイスのために他の USB 周辺機器の接続解除を強制するかもしれない．

<!-- * Limited availability of a direct computer interface like a USB port could pose usability difficulties. For example, laptop computers often have a limited number of USB ports, which may force users to unplug other USB peripherals to use the single-factor cryptographic device. -->

#### 10.2.8 Multi-Factor 暗号デバイス

<!-- #### 10.2.8 Multi-Factor Cryptographic Software -->

**_代表的な使用法_**

<!-- **_Typical Usage_** -->

Authenticate するために，ユーザはディスク上に格納された Cryptographic Key，またはアクティベーションを必要とするなんらかの"ソフト"メディアの所有とコントロールを証明する． アクティベーションは，Memorized Secret または Biometrics いずれかの第二の Authentication Factor のインプットによるものである． 追加の要素に対する Usability の考慮事項も同様に適用される． — Multi-Factor Authenticator に使用される Memorized Secret については [セクション 10.2.1](#memorizedsecrets)，Biometrics については [Section 10.4](#biomusability) を参照．

<!-- In order to authenticate, users prove possession and control of the cryptographic key stored on disk or some other "soft" media that requires activation. The activation is through the input of a second authentication factor, either a memorized secret or a biometric. Usability considerations for the additional factor apply as well — see [Section 10.2.1](#memorizedsecrets) for memorized secrets and [Section 10.4](#biomusability) for biometrics used in multi-factor authenticators. -->

代表的な使用法のための Usability の考慮事項は以下を含む :

<!-- Usability considerations for typical usage include: -->

* ユーザがどの Authentication タスクに使用する Cryptographic Key であるかを認識して呼び出す必要があるため，Cryptographic Key にはユーザにとって意味のある適切で説明的な名前がつけられる． これはユーザが，似ている，またはあいまいな名前の複数の Cryptographic Key を扱うことを予防する． より小さなモバイルデバイス上の複数の Cryptographic Key から選択することは，画面サイズが小さくなるために Cryptographic Key の名前が短縮されると特に問題になる．

<!-- * Give cryptographic keys appropriately descriptive names that are meaningful to users since users have to recognize and recall which cryptographic key to use for which authentication task. This prevents users from having to deal with multiple similarly- and ambiguously-named cryptographic keys. Selecting from multiple cryptographic keys on smaller mobile devices may be particularly problematic if the names of the cryptographic keys are shortened due to reduced screen size. -->

#### 10.2.9 Multi-Factor 暗号デバイス

<!-- #### 10.2.9 Multi-Factor Cryptographic Device -->

**_代表的な使用法_**

<!-- **_Typical Usage_** -->

ユーザは Multi-Factor 暗号デバイスの所持と保護された Cryptographic Key のコントロールを証明することで Authenticate する． デバイスは，Memorized Secret または Biometrics いずれかの第二の Authentication Factor によってアクティベートされる． 追加の要素に対する Usability の考慮事項も同様に適用される． — Multi-Factor Authenticator に使用される Memorized Secret については [セクション 10.2.1](#memorizedsecrets)，Biometrics については [Section 10.4](#biomusability) を参照．

<!-- Users authenticate by proving possession of the multi-factor cryptographic device and control of the protected cryptographic key. The device is activated by a second authentication factor, either a memorized secret or a biometric. Usability considerations for the additional factor apply as well — see [Section 10.2.1](#memorizedsecrets) for memorized secrets and [Section 10.4](#biomusability) for biometrics used in multi-factor authenticators. -->

代表的な使用法のための Usability の考慮事項は以下を含む :

<!-- Usability considerations for typical usage include: -->

* ユーザは，認証のあと，Multi-Factor 暗号デバイスを接続したままにすることを必要としない． ユーザはそれが終わったあと，Multi-Factor 暗号デバイスを取り外すことを忘れるかもしれない ( 例 : スマートカードリーダー内にスマートカードを忘れてコンピュータから遠ざかるなど ) ．

  * ユーザは Multi-Factor 暗号デバイスが接続されたままである必要があるのかどうか，知らされることが求められる．

<!-- * Do not require users to keep multi-factor cryptographic devices connected following authentication. Users may forget to disconnect the multi-factor cryptographic device when they are done with it (e.g., forgetting a smartcard in the smartcard reader and walking away from the computer).
  * Users need to be informed regarding whether the multi-factor cryptographic device is required to stay connected or not. -->

* ユーザがどの Authentication タスクに使用する Cryptographic Key であるかを認識して呼び出す必要があるため，Cryptographic Key にはユーザにとって意味のある適切で説明的な名前がつけられる． これはユーザが，似ている，またはあいまいな名前の複数の Cryptographic Key に面することを予防する． スマートフォンのような，より小さなモバイルデバイス上の複数の Cryptographic Key から選択することは，画面サイズが小さくなるために Cryptographic Key の名前が短縮されると特に問題になる．

<!-- * Give cryptographic keys appropriately descriptive names that are meaningful to users since users have to recognize and recall which cryptographic key to use for which authentication task. This prevents users being faced with multiple similarly and ambiguously named cryptographic keys. Selecting from multiple cryptographic keys on smaller mobile devices (such as smartphones) may be particularly problematic if the names of the cryptographic keys are shortened due to reduced screen size. -->

* USB ポートのような直接的コンピュータインターフェースの利用が限られる場合は Usability の問題を引き起こすことがある． たとえば，ラップトップコンピュータの USB ポートの数はしばしばとても限られるため，ユーザに対して Multi-Factor 暗号デバイスのために他の USB 周辺機器の接続解除を強制するかもしれない．

<!-- * Limited availability of a direct computer interface like a USB port could pose usability difficulties. For example, laptop computers often have a limited number of USB ports, which may force users to unplug other USB peripherals to use the multi-factor cryptographic device. -->

### 10.3 ユーザビリティに関する考慮事項のまとめ

<!-- ### 10.3 Summary of Usability Considerations -->

[表10-1](#t10-1) に各 Authenticator Type の 代表的な使用法と断続的なイベントについての Usability の考慮事項 をまとめる． 代表的な使用法についての Usability の考慮事項の多くは，行で示されているように，ほとんどの Authenticator Type に適用される． この表では，Authenticator Type 全体にわたる Usability の共通で多様な特性をハイライトしている． 各列は，各 Authenticator に対処する Usability の属性を簡単に識別できるようにする． ユーザのゴールや使用状況により，特定の属性が他の属性よりも重視されることがある． 可能な限り，代替の Authenticator Type を提供し，ユーザがそれらを選択できるようにする．

<!-- [Table 10-1](#t10-1) summarizes the usability considerations for typical usage and intermittent events for each authenticator type. Many of the usability considerations for typical usage apply to most of the authenticator types, as demonstrated in the rows. The table highlights common and divergent usability characteristics across the authenticator types. Each column allows readers to easily identify the usability attributes to address for each authenticator. Depending on users' goals and context of use, certain attributes may be valued over others. Whenever possible, provide alternative authenticator types and allow users to choose between them. -->

Multi-Factor Authenticator ( 例 : Multi-Factor OTP デバイス，Multi-Factor 暗号ソフトウェア，Multi-Factor 暗号デバイスなど ) はまたそれらの第二要素の Usability の考慮事項 を継承する． Biometrics は Multi-Factor Authentication ソリューションのアクティベーション要素としてのみ許されているため，Biometrics の Usability の考慮事項は 表 10-1 には含まれず，セクション 10.4 で説明される．

<!-- Multi-factor authenticators (e.g., multi-factor OTP devices, multi-factor cryptographic software, and multi-factor cryptographic devices) also inherit their secondary factor's usability considerations. As biometrics are only allowed as an activation factor in multi-factor authentication solutions, usability considerations for biometrics are not included in Table 10-1 and are discussed in Section 10.4. -->

<a name="t10-1"></a>

<div class="text-center" markdown="1">

**表 10-1 Authenticator Type による Usability の考慮事項のまとめ**

<!-- **Table 10-1 Usability Considerations Summary by Authenticator Type** -->

![](sp800-63b/media/use.png)

</div>

### <a name="biomusability"></a> 10.4 Biometrics の Usability の考慮事項

<!-- ### <a name="biomusability"></a> 10.4 Biometrics Usability Considerations -->

このセクションは Biometrics の 一般的な Usability の考慮事項についてのハイレベルオーバービューを提供する． Biometrics Usability のより詳細な説明は *Usability & Biometrics, Ensuring Successful Biometric Systems* [NIST Usability](#use-and-bio) に見つけることができる．

<!-- This section provides a high-level overview of general usability considerations for biometrics. A more detailed discussion of biometric usability can be found in *Usability & Biometrics, Ensuring Successful Biometric Systems* [NIST Usability](#use-and-bio). -->

ほかの Biometrics 様式も存在するが，以下の3つの Biometrics 様式が Authentication のためによく利用される : 指紋，顔，虹彩

<!-- Although there are other biometric modalities, the following three biometric modalities are more commonly used for authentication: fingerprint, face and iris. -->

**_代表的な使用法_**

<!-- **_Typical Usage_** -->

* すべての様式で，ユーザのなじみ深さと熟練がデバイスのパフォーマンスを向上させる．

<!-- * For all modalities, user familiarity and practice with the device improves performance. -->

* デバイスアフォーダンス ( すなわち，ユーザがアクションを実行することを可能にするデバイスの特性 )，フィードバック，および明確な指示は Biometrics デバイスによるユーザの成功にとって極めて重要である． たとえば，生存検出のために必要なアクションについての明確な指示を提供する．

<!-- * Device affordances (i.e., properties of a device that allow a user to perform an action), feedback, and clear instructions are critical to a user's success with the biometric device. For example, provide clear instructions on the required actions for liveness detection. -->

* 理想的には，ユーザは第二の Authentication Factor のために，最も快適な様式を選択することができる． ユーザ人口は，他よりもいくつかの Biometrics 様式をより快適かつなじみ深く感じ，また受け入れるだろう．

<!-- * Ideally, users can select the modality they are most comfortable with for their second authentication factor. The user population may be more comfortable and familiar with — and accepting of — some biometric modalities than others. -->

* Biometrics をアクティベーション要素とするユーザ体験
  * 残された試行回数について，明確で有意義なフィードバックを提供する． たとえば，混乱と不満を軽減するため，レート制限 (すなわち，スロットリング) によって次の試行までに待たなければならない時間を知らせる．

<!-- * User experience with biometrics as an activation factor.
  * Provide clear, meaningful feedback on the number of remaining allowed attempts. For example, for rate limiting (i.e., throttling), inform users of the time period they have to wait until next attempt to reduce user confusion and frustration. -->

* 指紋の Usability の考慮事項:
  * ユーザは最初の Enrollment に使用した指を覚えておく必要がある．
  * 指の湿気の量はセンサーのキャプチャー成功能力に影響する．
  * 指紋採取の品質に影響を及ぼす追加の要素には，年齢，性別，職業がある ( 例 : 化学薬品を取り扱う，または手で広範囲に働くユーザは摩擦隆線が低下している可能性がある ) ．

<!-- * Fingerprint Usability Considerations:
  * Users have to remember which finger(s) they used for initial enrollment.
  * The amount of moisture on the finger(s) affects the sensor's ability for successful capture.
  * Additional factors influencing fingerprint capture quality include age, gender, and occupation (e.g., users handling chemicals or working extensively with their hands may have degraded friction ridges). -->

* 顔の Usability の考慮事項 :
  * 顔認識の精度に影響するため，ユーザは Enrollment の際にアーティファクト ( 例 : 眼鏡など ) を装着したかどうかを覚えておく必要がある．
  * 環境の証明条件の違いは顔認識の精度に影響することがある．
  * 顔の表情は顔認識の精度に影響する ( 例 : 笑顔に対するナチュラルな状態 ) ．
  * 顔のポーズは顔認識の精度に影響する ( 例 : カメラを見下ろす，またはカメラから遠ざかる ) ．

<!-- * Face Usability Considerations:
  * Users have to remember whether they wore any artifacts (e.g., glasses) during enrollment because it affects facial recognition accuracy.
  * Differences in environmental lighting conditions can affect facial recognition accuracy.
  * Facial expressions affect facial recognition accuracy (e.g., smiling versus neutral expression).
  * Facial poses affect facial recognition accuracy (e.g., looking down or away from the camera). -->

* 虹彩の Usability の考慮事項 :
  * カラーコンタクトの装着は虹彩認識精度に影響することがある．
  * 目の手術を受けたユーザは手術後の再登録が必要になることがある．
  * 環境の証明条件の違いは虹彩認識精度，特に特定の虹彩の色に影響することがある．

<!-- * Iris Usability Considerations:
  * Wearing colored contacts may affect the iris recognition accuracy.
  * Users who have had eye surgery may need to re-enroll post-surgery.
  * Differences in environmental lighting conditions can affect iris recognition accuracy, especially for certain iris colors. -->

**_断続的なイベント_**
<!-- **_Intermittent Events_** -->

Biometrics は Multi-Factor Authentication の第二の要素としてのみ許可されているため，第一の要素の断続的なイベントの Usability の考慮事項は引き続き適用される． 認識精度に影響することがある Biometrics 使用の断続的なイベントは以下のものを含んでいるが，これに限らない :

<!-- As biometrics are only permitted as a second factor for multi-factor authentication, usability considerations for intermittent events with the primary factor still apply. Intermittent events with biometrics use include, but are not limited to, the following, which may affect recognition accuracy: -->

* ユーザが登録した指にけがをすると，指紋認識が機能しないことがある．指紋が薄くなったユーザにとっては指紋 Authentication は困難である．
* Authentication のための顔認識の時点と最初の Enrollment の時点の間に経過した時間は，ユーザの顔の経年による自然の変化として認識精度に影響することがある． ユーザの体重の変化もまた要因となる．
* 眼科手術を受けた人は，再登録を行わない限り虹彩認識が機能しないことがある．

<!-- * If users injure their enrolled finger(s), fingerprint recognition may not work. Fingerprint authentication will be difficult for users with degraded fingerprints.
* The time elapsed between the time of facial recognition for authentication and the time of the initial enrollment can affect recognition accuracy as a user's face changes naturally over time. A user's weight change may also be a factor.
* Iris recognition may not work for people who had eye surgery, unless they re-enroll. -->

すべての Biometrics 様式において，断続的なイベントの Usability の考慮事項は以下を含む :

<!-- Across all biometric modalities, usability considerations for intermittent events include: -->

* 代替の Authentication 方法が利用可能で機能している必要がある．Biometrics が機能しない場合，代替の第二要素としてユーザが Memorized Secret を使用できるようにする．
* 技術的な支援のための規定 :
  * どのようにして，どこで技術的な支援を得られるかの情報を明確に伝える． たとえば，オンラインセルフサービス機能へのリンク，ヘルプデスクサポートのための電話番号などの情報をユーザに提供する． 理想的には，外部からの介入なしにユーザが断続的なイベントから自分たちで回復できるだけの十分な情報を提供する．
  * Biometrics センサの感度に影響することがある要因をユーザに通知する ( 例 : センサーの清浄度 ) ．

## 11 参照
*This section is informative.*

### 11.1 General References

<a name="balloon"></a>[BALLOON] Boneh, Dan, Corrigan-Gibbs, Henry,  and Stuart Schechter. "Balloon Hashing: A Memory-Hard Function Providing Provable Protection Against Sequential Attacks," *Asiacrypt 2016*, October, 2016. Available at: <https://eprint.iacr.org/2016/027>.

<a name="blacklists"></a>[Blacklists] Habib, Hana, Jessica Colnago, William Melicher, Blase Ur, Sean Segreti, Lujo Bauer, Nicolas Christin, and Lorrie Cranor. "Password Creation in the Presence of Blacklists," 2017. Available at: <https://www.internetsociety.org/sites/default/files/usec2017_01_3_Habib_paper.pdf>

<a name="composition"></a>[Composition] Komanduri, Saranga, Richard Shay, Patrick Gage Kelley, Michelle L Mazurek, Lujo Bauer, Nicolas Christin, Lorrie Faith Cranor, and Serge Egelman. “Of Passwords and People: Measuring the Effect of Password-Composition Policies.” In Proceedings of the SIGCHI Conference on Human Factors in Computing Systems, 2595–2604. ACM, 2011. Available at: <https://www.ece.cmu.edu/~lbauer/papers/2011/chi2011-passwords.pdf>.

<a name="E-Gov"></a>[E-Gov] *E-Government Act* \[includes FISMA] (P.L. 107-347), December 2002, available at: <http://www.gpo.gov/fdsys/pkg/PLAW-107publ347/pdf/PLAW-107publ347.pdf>.

<a name="EO13681"></a>[EO 13681] Executive Order 13681, *Improving the Security of Consumer Financial Transactions*, October 17, 2014, available at: <https://www.federalregister.gov/d/2014-25439>.

<a name="FEDRAMP"></a>[FEDRAMP] General Services Administration, *Federal Risk and Authorization Management Program*, available at: <https://www.fedramp.gov/>.

<a name="ICAM"></a>[ICAM] National Security Systems and Identity, Credential and Access Management Sub-Committee Focus Group, Federal CIO Council, *ICAM Lexicon*, Version 0.5, March 2011.

<a name="M-03-22"></a>[M-03-22] OMB Memorandum M-03-22, *OMB Guidance for Implementing the Privacy Provisions of the E-Government Act of 2002*, September 26, 2003, available at: <https://georgewbush-whitehouse.archives.gov/omb/memoranda/m03-22.html>.

<a name="M-04-04"></a>[M-04-04] OMB Memorandum M-04-04, *E-Authentication Guidance for Federal Agencies*, December 16, 2003, available at: <https://georgewbush-whitehouse.archives.gov/omb/memoranda/fy04/m04-04.pdf>.

<a name="meters"></a>[Meters] de Carné de Carnavalet, Xavier and Mohammad Mannan. "From Very Weak to Very Strong: Analyzing Password-Strength Meters." In Proceedings of the Network and Distributed System Security Symposium (NDSS), 2014. Available at: <http://www.internetsociety.org/sites/default/files/06_3_1.pdf>

<a name="use-and-bio"></a>[NIST Usability] National Institute and Standards and Technology, *Usability & Biometrics, Ensuring Successful Biometric Systems*, June 11, 2008, available at: <http://www.nist.gov/customcf/get_pdf.cfm?pub_id=152184>.

<a name="OWASP-session"></a>[OWASP-session] Open Web Application Security Project, *Session Management Cheat Sheet*, available at: <https://www.owasp.org/index.php/Session_Management_Cheat_Sheet>.

<a name="OWASP-XSS-prevention"></a>[OWASP-XSS-prevention] Open Web Application Security Project, *XSS (Cross Site Scripting) Prevention Cheat Sheet*, available at: <https://www.owasp.org/index.php/XSS_(Cross_Site_Scripting)_Prevention_Cheat_Sheet>.

<a name="persistence"></a>[Persistence] herley, cormac, and Paul van Oorschot. "A Research Agenda Acknowledging the Persistence of Passwords," IEEE Security&Privacy Magazine, 2012. Available at: <http://research.microsoft.com/apps/pubs/default.aspx?id=154077>.

<a name="PrivacyAct"></a>[Privacy Act] *Privacy Act of 1974* (P.L. 93-579), December 1974, available at: <https://www.justice.gov/opcl/privacy-act-1974>.

<a name="policies"></a>[Policies] Weir, Matt, Sudhir Aggarwal, Michael Collins, and Henry Stern. "Testing Metrics for Password Creation Policies by Attacking Large Sets of Revealed Passwords." In Proceedings of the 17th ACM Conference on Computer and Communications Security, 162–175. CCS ’10. New York, NY, USA: ACM, 2010. doi:10.1145/1866307.1866327.

<a name="Section508"></a>[Section 508] Section 508 Law and Related Laws and Policies (January 30, 2017), available at: <https://www.section508.gov/content/learn/laws-and-policies>.

<a name="shannon"></a>[Shannon] Shannon, Claude E. "A Mathematical Theory of Communication," *Bell System Technical Journal*, v. 27, pp. 379-423, 623-656, July, October, 1948.

<a name="strength"></a>[Strength] Kelley, Patrick Gage, Saranga Komanduri, Michelle L Mazurek, Richard Shay, Timothy Vidas, Lujo Bauer, Nicolas Christin, Lorrie Faith Cranor, and Julio Lopez. “Guess Again (and Again and Again): Measuring Password Strength by Simulating Password-Cracking Algorithms.” In Security and Privacy (SP), 2012 IEEE Symposium On, 523–537. IEEE, 2012. Available at: <http://ieeexplore.ieee.org/iel5/6233637/6234400/06234434.pdf>.


### 11.2 Standards

<a name="bcp195"></a>[BCP 195] Sheffer, Y., Holz, R., and P. Saint-Andre, *Recommendations for Secure Use of Transport Layer Security (TLS) and Datagram Transport Layer Security (DTLS)*, BCP 195, RFC 7525,DOI 10.17487/RFC7525, May 2015, <https://doi.org/10.17487/RFC7525>.

<a name="ISO9241"></a>[ISO 9241-11] International Standards Organization, ISO/IEC 9241-11 *Ergonomic requirements for office work with visual display terminals (VDTs) — Part 11: Guidance on usability*, March 1998, available at: <https://www.iso.org/standard/16883.html>.

<a name="ISOIEC2382-37"></a>[ISO/IEC 2382-37] International Standards Organization, *Information technology — Vocabulary — Part 37: Biometrics*, 2017, available at: <http://standards.iso.org/ittf/PubliclyAvailableStandards/c066693_ISO_IEC_2382-37_2017.zip>.

<a name="ISOIEC10646"></a>[ISO/IEC 10646] International Standards Organization, *Universal Coded Character Set*, 2014, available at: <http://standards.iso.org/ittf/PubliclyAvailableStandards/c063182_ISO_IEC_10646_2014.zip>.

<a name="ISO24745"></a>[ISO/IEC 24745] International Standards Organization, *Information technology — Security techniques — Biometric information protection*, 2011, available at: <http://www.iso.org/iso/iso_catalogue/catalogue_tc/catalogue_detail.htm?csnumber=52946>.

<a name="ISOIEC30107-1"></a>[ISO/IEC 30107-1] International Standards Organization, *Information technology — Biometric presentation attack detection — Part 1: Framework*, 2016, available at: <http://standards.iso.org/ittf/PubliclyAvailableStandards/c053227_ISO_IEC_30107-1_2016.zip>.

<a name="ISOIEC30107-3"></a>[ISO/IEC 30107-3] International Standards Organization, *Information technology — Biometric presentation attack detection — Part 3: Testing and reporting*, 2017.

<a name="RFC20"></a>[RFC 20] Cerf, V., *ASCII format for network interchange*, STD 80, RFC 20, DOI 10.17487/RFC0020, October 1969, <https://doi.org/10.17487/RFC0020>.

<a name="RFC5246"></a>[RFC 5246] IETF, *The Transport Layer Security (TLS) Protocol Version 1.2*, RFC 5246, DOI 10.17487/RFC5246, August 2008, <https://doi.org/10.17487/RFC5246>.

<a name="RFC5280"></a>[RFC 5280] IETF, *Internet X.509 Public Key Infrastructure Certificate and CRL Profile*, RFC 5280, DOI 10.17487/RFC5280, May 2008, <https://doi.org/10.17487/RFC5280>.

<a name="RFC6238"></a>[RFC 6238] IETF, *TOTP: Time-Based One-Time Password Algorithm*,RFC 6238, DOI 10.17487/RFC6238, <https://doi.org/10.17487/RFC6238>.

<a name="RFC6960"></a>[RFC 6960] IETF, *X.509 Internet Public Key Infrastructure Online Certificate Status Protocol - OCSP*, RFC 6960, DOI 10.17487/RFC6960, <https://doi.org/10.17487/RFC6960>.

<a name="UAX15"></a>[UAX 15] Unicode Consortium, *Unicode Normalization Forms*, Unicode Standard Annex 15, Version 9.0.0, February, 2016, available at: <http://www.unicode.org/reports/tr15/>.

### 11.3 NIST Special Publications

NIST 800 Series Special Publications are available at: <http://csrc.nist.gov/publications/nistpubs/index.html>. The following publications may be of particular interest to those implementing systems of applications requiring digital authentication.

<a name="SP800-38B"></a>[SP 800-38B] NIST Special Publication 800-38B, *Recommendation for Block Cipher Modes of Operation: the CMAC Mode for Authentication*, October, 2016, <http://dx.doi.org/10.6028/NIST.SP.800-38B>.

<a name="SP800-52"></a>[SP 800-52] NIST Special Publication 800-52 Revision 1, *Guidelines for the Selection, Configuration, and Use of Transport Layer Security (TLS) Implementations*, April, 2014, <http://dx.doi.org/10.6028/NIST.SP.800-52r1>

<a name="SP800-53"></a>[SP 800-53] NIST Special Publication 800-53 Revision 4, *Recommended Security and Privacy Controls for Federal Information Systems and Organizations*, April 2013 (updated January 22, 2015), <http://dx.doi.org/10.6028/NIST.SP.800-53r4>.

<a name="SP800-57P1"></a>[SP 800-57 Part 1] NIST Special Publication 800-57 Part 1, Revision 4, *Recommendation for Key Management, Part 1: General*, January 2016, <http://dx.doi.org/10.6028/NIST.SP.800-57pt1r4>.

<a name="SP800-63-3"></a>[SP 800-63-3] NIST Special Publication 800-63-3, *Digital Identity Guidelines*, June 2017, <https://doi.org/10.6028/NIST.SP.800-63-3>.

<a name="SP800-63A"></a>[SP 800-63A] NIST Special Publication 800-63A, *Digital Identity Guidelines: Enrollment and Identity Proofing Requirements*, June 2017, <https://doi.org/10.6028/NIST.SP.800-63a>.

<a name="SP800-63C"></a>[SP 800-63C] NIST Special Publication 800-63C, *Digital Identity Guidelines: Authentication and Lifecycle Management*, June 2017, <https://doi.org/10.6028/NIST.SP.800-63c>.

<a name="SP800-90Ar1"></a>[SP 800-90Ar1] NIST Special Publication 800-90A Revision 1, *Recommendation for Random Number Generation Using Deterministic Random Bit Generators*, June 2015, <http://dx.doi.org/10.6028/NIST.SP.800-90Ar1>.

<a name="SP800-107"></a>[SP 800-107] NIST Special Publication 800-107 Revision 1, *Recommendation for Applications Using Approved Hash Algorithms*, August 2012, <http://dx.doi.org/10.6028/NIST.SP.800-107r1>.

<a name="SP800-131A"></a>[SP 800-131A] NIST Special Publication 800-131A Revision 1, *Transitions: Recommendation for Transitioning the Use of Cryptographic Algorithms and Key Lengths*, November 2015, <http://dx.doi.org/10.6028/NIST.SP.800-131Ar1>

<a name="SP800-132"></a>[SP 800-132] NIST Special Publication 800-132, *Recommendation for Password-Based Key Derivation*, December 2010, <http://dx.doi.org/10.6028/NIST.SP.800-132>.

<a name="SP800-185"></a>[SP 800-185] NIST Special Publication 800-185, *SHA-3 Derived Functions: cSHAKE, KMAC, TupleHash, and ParallelHash*, December, 2016, <https://doi.org/10.6028/NIST.SP.800-185>.

### 11.4 Federal Information Processing Standards

<a name="FIPS140-2"></a>[FIPS 140-2] Federal Information Processing Standard Publication 140-2, *Security Requirements for Cryptographic Modules*, May 25, 2001 (with Change Notices through December 3, 2002), <https://doi.org/10.6028/NIST.FIPS.140-2>.

<a name="FIPS198-1"></a>[FIPS 198-1] Federal Information Processing Standard Publication 198-1, *The Keyed-Hash Message Authentication Code (HMAC)*, July 2008, <https://doi.org/10.6028/NIST.FIPS.198-1>.

<a name="FIPS201"></a>[FIPS 201] Federal Information Processing Standard Publication 201-2, *Personal Identity Verification (PIV) of Federal Employees and Contractors*, August 2013, <http://dx.doi.org/10.6028/NIST.FIPS.201-2>.

<a name="FIPS202"></a>[FIPS 202] Federal Information Processing Standard Publication 202, *SHA-3 Standard: Permutation-Based Hash and Extendable-Output Functions*, August 2015, <http://dx.doi.org/10.6028/NIST.FIPS.202>.
<a name="appA"></a>

## 付録 A&mdash;記憶シークレットの強度

*本付録は参考情報である.*
<!--
*This appendix is informative.*
-->

本付録を通じて，ディスカッションを容易にするため"パスワード"という単語を用いる．使用箇所では，パスワード同様にパスフレーズおよびPINを含むものとして解釈すべきである．
<!--
Throughout this appendix, the word "password" is used for ease of discussion. Where used, it should be interpreted to include passphrases and PINs as well as passwords.
-->

### A.1 Introduction

ユーザビリティとセキュリティの立場の両面からパスワードの利用は大きな不満を伴うにも関わらず，Authenticationの形式 [[Persistance]](#persistence) として極めて広く利用され続けている．人間は，しかしながら，複雑で任意のシークレットを記憶する能力が限られているため，しばしば容易に推測できるパスワードを選ぶことがある．その結果発生するセキュリティ上の問題に対処するため，オンラインサービスでは，これらの記憶シークレットの複雑さを増加させる努力として，ルールが導入されてきた．最も注目すべき形式としては，ユーザがパスワードを選択する際に，少なくとも1文字以上の数字，大文字，記号のような文字種を組み合わせて作成するよう求める構成ルールがある．しかし，漏洩したパスワードデータベースの分析により，そのようなルールでは，ユーザビリティと記憶難易度へ与える影響はあれど，当初 [[ポリシー]](#policies) で考えられていたほど得られる利益が大きくないことが明らかになった．
<!--
Despite widespread frustration with the use of passwords from both a usability and security standpoint, they remain a very widely used form of authentication [[Persistence]](#persistence). Humans, however, have only a limited ability to memorize complex, arbitrary secrets, so they often choose passwords that can be easily guessed. To address the resultant security concerns, online services have introduced rules in an effort to increase the complexity of these memorized secrets. The most notable form of these is composition rules, which require the user to choose passwords constructed using a mix of character types, such as at least one digit, uppercase letter, and symbol. However, analyses of breached password databases reveal that the benefit of such rules is not nearly as significant as initially thought [[Policies]](#policies), although the impact on usability and memorability is severe.
-->

ユーザが選択したパスワードの複雑さは，エントロピー [[Shannon]](#shannon) という情報理論の概念を用いて特徴づけられることが多い．確定的分布関数に従うデータに対するエントロピーを計算することは容易だが，ユーザが選択したパスワードのエントロピーを推測することは難しく，それを行うための過去の努力は特に正確ではない．このため，主にパスワードの長さの観点で差があり少しシンプルになっているアプローチがここに示されている．
<!--
Complexity of user-chosen passwords has often been characterized using the information theory concept of entropy [[Shannon]](#shannon). While entropy can be readily calculated for data having deterministic distribution functions, estimating the entropy for user-chosen passwords is difficult and past efforts to do so have not been particularly accurate. For this reason, a different and somewhat simpler approach, based primarily on password length, is presented herein.
-->

パスワードの利用に関連する多くの攻撃はパスワードの複雑さと長さに影響をうけない．キー入力ロギング，フィッシング，ソーシャルエンジニアリング攻撃は，複雑で長いパスワードに対しても，それがシンプルなものであるのと同様に作用する．これらの攻撃は本付録の対象外である．
<!--
Many attacks associated with the use of passwords are not affected by password complexity and length. Keystroke logging, phishing, and social engineering attacks are equally effective on lengthy, complex passwords as simple ones. These attacks are outside the scope of this Appendix.
-->

### A.2 長さ
<!--
### A.2 Length
-->

パスワードの長さは，パスワードの強度 [[Strength]](#strength) [[Composition]](#composition) を特徴付ける主要因であることがわかっている．短すぎるパスワードは単語や一般的に選択されたパスワードを利用する辞書攻撃と同様にブルートフォース攻撃をももたらす．
<!--
Password length has been found to be a primary factor in characterizing password strength [[Strength]](#strength) [[Composition]](#composition). Passwords that are too short yield to brute force attacks as well as to dictionary attacks using words and commonly chosen passwords.
-->

最低でもどの程度の長さのパスワードが必要となるかは，想定する脅威モデルによって大きく左右される．攻撃者がパスワードを推測してログインを試みるオンライン攻撃では，許容するログイン試行回数に対してレート制限をかけることで緩和することができる．攻撃者(またはしつこくタイプミスを繰り返すClaimant)が大量の誤ったパスワード推測を行いSubscriberに対するDoS攻撃を容易に行えないようにするためには，そこまで誤った試行回数が多くはないのであればレート制限を発生させなくても済むほど十分複雑である必要がある．とはいえ，推測が成功する確率が大きくなる前にレート制限は発生する．
<!--
The minimum password length that should be required depends to a large extent on the threat model being addressed. Online attacks where the attacker attempts to log in by guessing the password can be mitigated by limiting the rate of login attempts permitted. In order to prevent an attacker (or a persistent claimant with poor typing skills) from easily inflicting a denial-of-service attack on the subscriber by making many incorrect guesses, passwords need to be complex enough that rate limiting does not occur after a modest number of erroneous attempts, but does occur before there is a significant chance of a successful guess.
-->

オフライン攻撃は，攻撃者がデータベース侵害を通じて1つ以上のハッシュされたパスワードを得た場合に発生する可能性がある．攻撃者が1人以上のユーザのパスワードを決定する能力は，パスワードの記録方法によって異なる．一般的には，パスワードはランダム値を用いてソルトを追加したうえでハッシュ化され，そのために計算コストが高いアルゴリズムを用いることが望ましい．そのような基準でさえも，レート制限なしで毎秒何十億もの計算を行うことができる現在の攻撃者の能力に対抗するためには，パスワードはオンライン攻撃だけに対抗することを想定するよりも複雑で大きなオーダーである必要がある．
<!--
Offline attacks are sometimes possible when one or more hashed passwords is obtained by the attacker through a database breach. The ability of the attacker to determine one or more users' passwords depends on the way in which the password is stored. Commonly, passwords are salted with a random value and hashed, preferably using a computationally expensive algorithm. Even with such measures, the current ability of attackers to compute many billions of hashes per second with no rate limiting requires passwords intended to resist such attacks to be orders of magnitude more complex than those that are expected to resist only online attacks.
-->

ユーザは，こういった理由もあり，望みどおり長いパスワードが使えるよう促進されるべきである．ハッシュされたパスワードの長さはパスワードそのものの長さと無関係であるため，長いパスワード(またはパスフレーズ)の利用を許可しない理由はない．極端に長いパスワード(おそらくはメガバイト)はハッシュ処理に極端に時間を要すると考えられるため，何らかの制限を行うのは適切なことである．
<!--
Users should be encouraged to make their passwords as lengthy as they want, within reason. Since the size of a hashed password is independent of its length, there is no reason not to permit the use of lengthy passwords (or pass phrases) if the user wishes. Extremely long passwords (perhaps megabytes in length) could conceivably require excessive processing time to hash, so it is reasonable to have some limit.
-->

### A.3 複雑さ
<!--
### A.3 Complexity
-->

上記のように，構成ルールはユーザが選択したパスワードの推測難易度を上げる目的で一般的に利用される．しかしながら，研究者はユーザが構成ルール [[Policies]](#policies) で課せられた要件に対してかなり予測可能な方法で応答することを示した．例えば，パスワードとして"password"を選択したユーザは，大文字と数字を含む要件を与えると，比較的高い確率で"Password1"を選択し，記号を要件に追加すると"Password1!"を選択する．
<!--
As noted above, composition rules are commonly used in an attempt to increase the difficulty of guessing user-chosen passwords. Research has shown, however, that users respond in very predictable ways to the requirements imposed by composition rules [[Policies]](#policies). For example, a user that might have chosen "password" as their password would be relatively likely to choose "Password1" if required to include an uppercase letter and a number, or "Password1!" if a symbol is also required.
-->

複雑なパスワードを作成しようとしてオンラインサービスにそれを拒否されるとユーザも不満を抱く．多くのサービスではスペースや様々な特殊文字を含むパスワードを拒否する．場合により，特殊文字が受け入れられないことがあるが，それは特殊文字を用いるSQLインジェクションのような攻撃を回避するためである．しかし，適切にハッシュ化されたパスワードはいかなる場合でもそのままデータベースに送信されることはないので，そのような予防策は不要である．ユーザはフレーズの利用を許容するためにスペースを使ってもよい．しかし，スペースはそれ自体ではパスワードの複雑さを僅かに増加させるにすぎず，ユーザビリティの問題(例えば，1つの場合に比べて2つのスペースを利用したことが検出されないなど)を招く可能性があるため，検証時は事前に入力されたパスワードから連続するスペースを削除すると効果的である．
<!--
Users also express frustration when attempts to create complex passwords are rejected by online services. Many services reject passwords with spaces and various special characters. In some cases, the special characters that are not accepted might be an effort to avoid attacks like SQL injection that depend on those characters. But a properly hashed password would not be sent intact to a database in any case, so such precautions are unnecessary. Users should also be able to include space characters to allow the use of phrases. Spaces themselves, however, add little to the complexity of passwords and may introduce usability issues (e.g., the undetected use of two spaces rather than one), so it may be beneficial to remove repeated spaces in typed passwords prior to verification.
-->

ユーザのパスワードの選択は非常に予想可能であり，攻撃者は過去に成功した正しいパスワードを推測する可能性がある．これらのパスワードには，辞書の単語や上記例の"Password1!"のような過去に漏洩したパスワード，このような状況から，ユーザが選択したパスワードは許容できないパスワードのブラックリストと比較することが望ましい．このブラックリストは，ユーザが選択する可能性が高い，過去に漏洩したパスワード，語彙集，辞書の単語および特定の用語(サービス自体の名前など)を含むべきである．ユーザのパスワードの選択は最低文字の要件が適用されるため，この辞書は要件に合致したものだけを含んでいる必要がある．
<!--
Users' password choices are very predictable, so attackers are likely to guess passwords that have been successful in the past. These include dictionary words and passwords from previous breaches, such as the "Password1!" example above. For this reason, it is recommended that passwords chosen by users be compared against a "black list" of unacceptable passwords. This list should include passwords from previous breach corpuses, dictionary words, and specific words (such as the name of the service itself) that users are likely to choose. Since user choice of passwords will also be governed by a minimum length requirement, this dictionary need only include entries meeting that requirement.
-->

極端に複雑な記憶シークレットは新たな潜在的な脆弱性を生み出す: シークレットを記憶できる可能性が減り，書き留めたり安全でない方法で電子的に記録する可能性が高まる．これらの慣例は必ずしも脆弱であるわけではないが，統計的にそのようなシークレットの記録方法のなかには脆弱なものがある．これは，極端に長い、または極端に複雑な記憶シークレットを要求しないということの追加の動機付けになる．
<!--
Highly complex memorized secrets introduce a new potential vulnerability: they are less likely to be memorable, and it is more likely that they will be written down or stored electronically in an unsafe manner. While these practices are not necessarily vulnerable, statistically some methods of recording such secrets will be. This is an additional motivation not to require excessively long or complex memorized secrets.
-->

### A.4 ランダムに選択されたシークレット
<!--
### A.4 Randomly-Chosen Secrets
-->

記憶シークレットの強度を決定するもう一つの要素が，それが生成されるプロセスである．(VerifierやCSPでは殆どの場合)分布が一様でランダムなシークレットでは、同じ長さと複雑さ要件に対してユーザが選択するシークレットに比べてパスワード推測やブルートフォース攻撃の難易度が高くなるだろう．従って，SP 800-63-2のLOA2ではランダムに選択された6桁の数字のPINが許容されるのに対し，ユーザが選択したシークレットの場合は最低8文字という要件になっている．
<!--
Another factor that determines the strength of memorized secrets is the process by which they are generated. Secrets that are randomly chosen (in most cases by the verifier or CSP) and are uniformly distributed will be more difficult to guess or brute-force attack than user-chosen secrets meeting the same length and complexity requirements. Accordingly, at LOA2, SP 800-63-2 permitted the use of randomly generated PINs with 6 or more digits while requiring user-chosen memorized secrets to be a minimum of 8 characters long.
-->

上で論じたように，記憶シークレットの長さ要件に考慮された脅威モデルには，オンライン攻撃に対するレート制限が含まれているが，オフライン攻撃は含まれていない．この制約があるものの，6桁のランダムに生成されたPINは記憶シークレットに対しては引き続き適切であるとみなされる．
<!--
As discussed above, the threat model being addressed with memorized secret length requirements includes rate-limited online attacks, but not offline attacks. With this limitation, 6 digit randomly-generated PINs are still considered adequate for memorized secrets.
-->

### A.5 サマリ
<!--
### A.5 Summary
-->

ここで推奨されている内容を超えた長さと複雑さの要件は，記憶シークレットの難易度を大幅に増加させ，ユーザの不満を増加させる．結果としてユーザはしばしばこれらの制限を回避してしまうため，逆効果である． 更に，ブラックリスト，安全なハッシュストレージ，レート制限などの他の緩和策は，のブルートフォース攻撃をためにより効果的です．したがって，複雑さについて追加の要件が課されることはない．
<!--
Length and complexity requirements beyond those recommended here significantly increase the difficulty of memorized secrets and increase user frustration. As a result, users often work around these restrictions in a way that is counterproductive. Furthermore, other mitigations such as blacklists, secure hashed storage, and rate limiting are more effective at preventing modern brute-force attacks. Therefore, no additional complexity requirements are imposed.
-->

## 付録 B&mdash;定義と略語

*This section is normative.*

### B.1 Definitions

Authentication 分野で使われる用語は幅広く, 多くの用語は以前のバージョンの SP 800-63 と整合性を保っているものの, いくつかは本リビジョンから定義が変更になっている. 多くの用語に複数の定義がありうるため, 本ドキュメントにおける定義を明確にする必要がある.

<!-- A wide variety of terms is used in the realm of authentication. While many terms' definitions are consistent with earlier versions of SP 800-63, some have changed in this revision. Many of these terms lack a single, consistent definition, warranting careful attention to how the terms are defined here. -->

#### <a name="access"></a> Access

オンラインのデジタルサービスの1つ以上の個別機能に接続すること.

<!-- To make contact with one or more discrete functions of an online, digital service. -->

#### Active Attack

Attacker が Claimant, Credential Service Provider (CSP), Verifier, Relying Party (RP) に対してデータを送信するような Authentication Protocol における Attack のこと. Active Attack の例としては Man-in-the-Middle Attack (MitM), Impersonation, Session Hijacking などが挙げられる.

<!-- An attack on the authentication protocol where the attacker transmits data to the claimant, Credential Service Provider (CSP), verifier, or Relying Party (RP). Examples of active attacks include man-in-the-middle (MitM), impersonation, and session hijacking. -->

#### Address of Record

許可された手段によって特定個人とのコミュニケーションに利用可能な, 有効かつ検証済の (物理的またはデジタルの) 場所情報.

<!-- The validated and verified location (physical or digital) where an individual can receive communications using approved mechanisms. -->

#### Applicant

Enrollment および Identity Proofing のプロセスを受けている主体.

<!-- A subject undergoing the processes of enrollment and identity proofing. -->

#### <a name="approved"></a> Approved Cryptography

Federal Information Processing Standard (FIPS) の承認, もしくは NIST の推奨を受けているもの. FIPS ないしは NIST Recommendation に (1) 指定されているか, (2) 採用されているアルゴリズムおよびテクニック.

<!-- Federal Information Processing Standard (FIPS)-approved or NIST recommended. An algorithm or technique that is either 1) specified in a FIPS or NIST Recommendation, or 2) adopted in a FIPS or NIST Recommendation. -->

#### Assertion

Verifier から Relying Party (RP) に対して送られる, Subscriber の Identity 情報を含んだ Statement. Assertion は検証済属性情報を含むこともある.

<!-- A statement from a verifier to an RP that contains information about a subscriber. Assertions may also contain verified attributes. -->

#### Assertion Reference

Assertion と紐付けて生成されるデータオブジェクトであり, Verifier を識別するとともに, Verifier が所有する Full Assertion へのポインタとして機能する.

<!-- A data object, created in conjunction with an assertion, that identifies the verifier and includes a pointer to the full assertion held by the verifier. -->

#### Asymmetric Keys

Public Key と Private Key からなる鍵ペア.
暗号化と復号, 署名生成と署名検証など, 対となるオペレーションに用いられる.

<!-- Two related keys, comprised of a public key and a private key, that are used to perform complementary operations such as encryption and decryption or signature verification and generation. -->

#### Attack

Authorize されていない主体が, Verifier や RP をだまして当該個人を Subscriber だと信じ込ませようとする行為.

<!-- An unauthorized entity's attempt to fool a verifier or RP into believing that the unauthorized individual in question is the subscriber. -->

#### Attacker

不正な意図を持ち情報システムに不正アクセスする主体. 内部犯も含む.

<!-- A party, including an insider, who acts with malicious intent to compromise a system. -->

#### Attribute

人や物に関する生来の性質や特徴.

<!-- A quality or characteristic ascribed to someone or something. -->

#### Attribute Bundle

パッケージ化された Attribute の集合で, 通常は単一の Assertion に含まれる. Attribute Bundle により, RP は関連する必要な Attribute 一式を IdP から簡単に受け取ることができる. Attribute Bundle は OpenID Connect の scope [[OpenID Connect Core 1.0]](#OpenIDConnectCore) と同義である.

<!-- A packaged set of attributes, usually contained within an assertion. Attribute bundles offer RPs a simple way to retrieve the most relevant attributes they need from IdPs. Attribute bundles are synonymous with OpenID Connect scopes [[OpenID Connect Core 1.0]](#OpenIDConnectCore). -->

#### Attribute Reference

Subscriber のプロパティーを Assert する Statement である. 必ずしも Identity 情報を含む必要はなく, フォーマットは問わない. 例えば, "birthday" という Attribute に対しては, "older than 18" や "born in December" などが Attribute Reference たりうる.

<!-- A statement asserting a property of a subscriber without necessarily containing identity information, independent of format. For example, for the attribute "birthday," a reference could be "older than 18" or "born in December." -->

#### Attribute Value

Subscriber のプロパティーを Assert する完全な Statement. フォーマットは問わない. 例えば "birthday" という Attribute に対しては, "12/1/1980" や "December 1, 1980" などが Attribute Value となりうる.

<!-- A complete statement asserting a property of a subscriber, independent of format. For example, for the attribute "birthday," a value could be "12/1/1980" or "December 1, 1980." -->

#### Authenticate

[Authentication](#authentication) 参照.

<!-- See [Authentication](#authentication). -->

#### Authenticated Protected Channel

接続元 (Client) が 接続先 (Server) を Authenticate しており, Approved Cryptography を用いて暗号化されたコミュニケーションチャネル. Authenticated Protocol Channel は機密性および MitM 保護を提供するものであり, ユーザーの Authentication プロセスの中でよく使われるものである. Transport Layer Security (TLS) [[BCP 195]](#bcp195) がその例としてあげられ, TLS では接続先が提示した Certificate を接続元が検証することになる. 特に指定がない限り, Authenticated Protected Channel では Server が Client を Authenticate する必要はない. Server の Authentication は, 各 Server 個別の対応ではなく, しばしば Trusted Root から始まる Certificate Chain を用いて行われる.

<!-- An encrypted communication channel that uses approved cryptography where the connection initiator (client) has authenticated the recipient (server). Authenticated protected channels provide confidentiality and MitM protection and are frequently used in the user authentication process. Transport Layer Security (TLS) [[BCP 195]](#bcp195) is an example of an authenticated protected channel where the certificate presented by the recipient is verified by the initiator. Unless otherwise specified, authenticated protected channels do not require the server to authenticate the client. Authentication of the server is often accomplished through a certificate chain leading to a trusted root rather than individually with each server. -->

#### <a name="authentication"></a> Authentication

ユーザー, プロセス, デバイスなどの Identity を検証すること. しばしばあるシステムのリソースへの Access を許可する際の必須要件となる.

<!-- Verifying the identity of a user, process, or device, often as a prerequisite to allowing access to a system's resources. -->

#### <a name="af"></a> Authentication Factor

*something you know*, *something you have*, および *something you are* という3種類の Authentication Factor がある. 全ての Authenticator はこれら1つ以上の Authentication Factor を持つ.

<!-- The three types of authentication factors are *something you know*, *something you have*, and *something you are*. Every authenticator has one or more authentication factors. -->

#### Authentication Intent

ユーザーを Authentication フローに介在させるプロセスを経ることによって, Claimant が Authenticate ないしは Reauthenticate を行う意思を確認するプロセス. Authenticator によっては, Authentication Intent をオペレーションの一部に含むこともあれば (e.g., OTP デバイス), ボタンを押させるなどといった特別なステップを要求するものもある. Authentication Intent は, 当該 Endpoint を Proxy として, マルウェアが Subscriber に気づかれずに Attacker を Authenticate してしまうケースに対する対抗策となる.

<!-- The process of confirming the claimant's intent to authenticate or reauthenticate by including a process requiring user intervention in the authentication flow. Some authenticators (e.g., OTP devices) establish authentication intent as part of their operation, others require a specific step, such as pressing a button, to establish intent. Authentication intent is a countermeasure against use by malware of the endpoint as a proxy for authenticating an attacker without the subscriber's knowledge. -->

#### Authentication Protocol

Claimant が正規の Authenticator の所有および管理権限を示すことで自身の Identity を確立するプロセスにおいて, Claimant と Verifier の間でやりとりされる一連のメッセージの定義. Claimant が意図した Verifier とコミュニケーションしていることを立証するためのプロセスを含むこともある.

<!-- A defined sequence of messages between a claimant and a verifier that demonstrates that the claimant has possession and control of one or more valid authenticators to establish their identity, and, optionally, demonstrates that the claimant is communicating with the intended verifier. -->

#### Authentication Protocol Run

Claimant と Verifier との間で Authentication (または Authentication 失敗) という結果に至るまでの, 一連のメッセージ交換.

<!-- An exchange of messages between a claimant and a verifier that results in authentication (or authentication failure) between the two parties. -->

#### Authentication Secret

あらゆる鍵を示す一般的な呼び名. Authentication Protocol において Attacker が Subscriber になりすますために利用することもできる.

<!-- A generic term for any secret value that an attacker could use to impersonate the subscriber in an authentication protocol. -->

Authentication Secret は *short-term authentication secrets* と *long-term authentication secrets* に分類することができ, 前者は限定的な期間のみ利用可能なもの, 後者は手動でリセットされるまで使い続けられるものを示す. Authenticator Secret は long-term authentication secret の代表的な例であり, Authenticator の出力する鍵が Authenticator Secret と異なる場合, その出力された鍵は一般的に short-term authentication secret である.

<!-- These are further divided into *short-term authentication secrets*, which are only useful to an attacker for a limited period of time, and *long-term authentication secrets*, which allow an attacker to impersonate the subscriber until they are manually reset. The authenticator secret is the canonical example of a long-term authentication secret, while the authenticator output, if it is different from the authenticator secret, is usually a short-term authentication secret. -->

#### <a name="authenticator"></a> Authenticator

Claimant が所有および管理するもので, 典型的な例としては暗号モジュールやパスワード等がある. Authenticator は Claimant の Identity を Authenticate するために用いられる. SP 800-63 の前エディションまでは *token* と呼ばれていたものと同等である.

<!-- Something the claimant possesses and controls (typically a cryptographic module or password) that is used to authenticate the claimant's identity. In previous editions of SP 800-63, this was referred to as a *token*. -->

#### Authenticator Assurance Level (AAL)

Authentication プロセスの強度を示すカテゴリー.

<!-- A category describing the strength of the authentication process. -->

#### <a name="authenticatoroutput"></a> Authenticator Output

Authenticator によって生成される出力値. 必要に応じて正当な Authenticator Output を生成することで, Claimant が Authenticator を所有し管理していることを証明できる. Verifier へ送信されるプロトコルメッセージは Authenticator Output によって異なり, プロトコルメッセージが明示的に Authenticator Output を含んでいることもあればそうでないこともある.

<!-- The output value generated by an authenticator. The ability to generate valid authenticator outputs on demand proves that the claimant possesses and controls the authenticator. Protocol messages sent to the verifier are dependent upon the authenticator output, but they may or may not explicitly contain it. -->

#### <a name="authenticatorsecret"></a> Authenticator Secret

Authenticator に内包されるシークレット値.

<!-- The secret value contained within an authenticator. -->

#### Authenticator Type

共通の特徴を持つ Authenticator のカテゴリー. Authenticator Type によっては単一の Authentication Factor のみを持つものもあれば, 2つの Authentication Factor を持つものもある.

<!-- A category of authenticators with common characteristics. Some authenticator types provide one authentication factor, others provide two. -->

#### Authenticity

データが意図された情報源から得られたものであるというプロパティー.

<!-- The property that data originated from its purported source. -->

#### Authoritative Source

Identity Evidence の Issuing Source がもつ正確な情報に Access できる, もしくは検証済みコピーを所有している主体. これにより Identity Proofing 実施時に CSP が Applicant の提出した Identity Evidence の正当性を確認できる. Issuing Source 自身が Authoritative Source であることもありうる. Authoritative Source は, Identity Proofing の検証フェーズの前に, 機関や CSP のポリシーによって決定されることも多い.

<!-- An entity that has access to, or verified copies of, accurate information from an issuing source such that a CSP can confirm the validity of the identity evidence supplied by an applicant during identity proofing. An issuing source may also be an authoritative source. Often, authoritative sources are determined by a policy decision of the agency or CSP before they can be used in the identity proofing validation phase. -->

#### Authorize

[access](#access) を許可するかどうかの判断. 通常は Subject の Attribute を評価することで自動的に判断される.

<!-- A decision to grant [access](#access), typically automated by evaluating a subject's attributes. -->

#### Back-Channel Communication

2つのシステム間で直接コネクションを貼って行われるコミュニケーション (標準プロトコルレベルでのプロキシの利用を許容する). ブラウザ等を媒介としたリダイレクトは用いない. HTTP Request & Response により実現される.

<!-- Communication between two systems that relies on a direct connection (allowing for standard protocol-level proxies), without using redirects through an intermediary such as a browser. This can be accomplished using HTTP requests and responses. -->

#### Bearer Assertion

ある主体が Identity を証明するために提示する Assertion であり, Assertion を保有していること自体が Assertion 持参人の Identity 証明として十分であるようなもの.

<!-- The assertion a party presents as proof of identity, where possession of the assertion itself is sufficient proof of identity for the assertion bearer. -->

#### Binding

Subscriber の Identity と Authenticator や Subscriber Session の間の関連性.

<!-- An association between a subscriber identity and an authenticator or given subscriber session. -->

#### Biometrics

個人の行動や生体情報をもとに個人を自動認識すること.

<!-- Automated recognition of individuals based on their biological and behavioral characteristics. -->

#### Challenge-Response Protocol

Verifier が Claimant に対してチャレンジ (通常はランダム値やノンス) を送信し, Claimant はシークレットと連結 (チャレンジと Shared Secret を一緒にハッシュしたり, チャレンジに対して Private Key による操作を実施) して応答を生成し, Verifier に返却するような Authentication Protocol. Verifier は Claimant が生成した応答を自身のみで検証 (チャレンジと Shared Secret のハッシュを再計算してレスポンスと比較したり, レスポンスに対して Public Key による操作を実施) し, Claimant がシークレットを所有し管理下に置いているを証明することができる.

<!-- An authentication protocol where the verifier sends the claimant a challenge (usually a random value or nonce) that the claimant combines with a secret (such as by hashing the challenge and a shared secret together, or by applying a private key operation to the challenge) to generate a response that is sent to the verifier. The verifier can independently verify the response generated by the claimant (such as by re-computing the hash of the challenge and the shared secret and comparing to the response, or performing a public key operation on the response) and establish that the claimant possesses and controls the secret. -->

#### Claimant

1つ以上の Authentication Protocol により Identity を検証される Subject.

<!-- A subject whose identity is to be verified using one or more authentication protocols. -->

#### Claimed Address

Subject により, 自分に到達可能だと Assert された物理的位置. 居住地の住所や郵便の届く住所などを含む.

<!-- The physical location asserted by a subject where they can be reached. It includes the individual's residential street address and may also include their mailing address. -->

例えば, 外国籍のパスポートを所持している状態で U.S. に在住する人物であれば, Identity Proofing プロセスにおいて住所を要求されることになる. そう言った場合の住所は, "address of record" ではなく "claimed address" となろう.

<!-- For example, a person with a foreign passport living in the U.S. will need to give an address when going through the identity proofing process. This address would not be an "address of record" but a "claimed address." -->

#### Claimed Identity

ある Applicant による, 個人に関する未検証かつ未証明な Attribute の申告.

<!-- An applicant's declaration of unvalidated and unverified personal attributes. -->

#### Completely Automated Public Turing test to tell Computers and Humans Apart (CAPTCHA)

利用者が人間か自動化されたエージェントかを区別するために Web フォームに追加する対話的な機能. 典型的には, 歪んだ画像や音声に対応するテキストの入力を求める方式である.

<!-- An interactive feature added to web forms to distinguish whether a human or automated agent is using the form. Typically, it requires entering text corresponding to a distorted image or a sound stream. -->

#### Credential

ある Identity および (任意で) 追加の Attribute を, 識別子を通じて, Subscriber が所有ないしは管理する Authenticator に紐付ける, 信頼のおけるオブジェクトもしくはデータ構造.

<!-- An object or data structure that authoritatively binds an identity - via an identifier or identifiers - and (optionally) additional attributes, to at least one authenticator possessed and controlled by a subscriber. -->

一般的に Credential は Subscriber に管理されることが想定されるが, 本ガイドライン群では Subscriber の Authenticator と Identity の紐付けを確立する CSP 管理下の電子レコードを指すこともある.

<!-- While common usage often assumes that the subscriber maintains the credential, these guidelines also use the term to refer to electronic records maintained by the CSP that establish binding between the subscriber's authenticator(s) and identity. -->

#### Credential Service Provider (CSP)

Subscriber の Authenticator を発行もしくは登録し, Subscriber に電子的な Credential を発行する信頼された主体. CSP は独立した第三者となることもあれば, 自身で発行した Credential を用いて自らサービスを提供することもある.

<!-- A trusted entity that issues or registers subscriber authenticators and issues electronic credentials to subscribers. A CSP may be an independent third party or issue credentials for its own use. -->

#### Cross-site Request Forgery (CSRF)

RP に対して Authenticate されている Subscriber が, セキュアな Session をつうじて Attacker のウェブサイトに接続する場合に発生する Attack であり, 加入者が無意識のうちに望まないアクションを RP 上で実行してしまうことになる.

<!-- An attack in which a subscriber currently authenticated to an RP and connected through a secure session browses to an attacker's website, causing the subscriber to unknowingly invoke unwanted actions at the RP. -->

<!--
For example, if a bank website is vulnerable to a CSRF attack, it may be possible for a subscriber to unintentionally authorize a large money transfer, merely by viewing a malicious link in a webmail message while a connection to the bank is open in another browser window.
-->

例えば, もし銀行のサイトが CSRF に対して脆弱である場合, 単にユーザが Web メールの本文中の悪意のあるリンクを参照するだけで, 別のブラウザウィンドウで銀行への接続が開かれ, 加入者が意図せず大きな金額の資金移動を Authorize してしまう可能性がある.

<!-- For example, if a bank website is vulnerable to a CSRF attack, it may be possible for a subscriber to unintentionally authorize a large money transfer, merely by viewing a malicious link in a webmail message while a connection to the bank is open in another browser window. -->

#### Cross-site Scripting (XSS)

Attacker が, 不正なコードを他の無害なページに対して注入することを許してしまう脆弱性. これらのスクリプトはターゲットのウェブサイトで生成されるスクリプトの権限で動作し, ウェブサイトとクライアント間でのデータ転送の Confidentiality (機密性) や Integrity (完全性) を侵害する. ウェブサイトは, ユーザが入力するデータをリクエストやフォームから受け取り, サニタイズを施して実行不可能にすることなく表示してしまう場合に脆弱である.

<!-- A vulnerability that allows attackers to inject malicious code into an otherwise benign website. These scripts acquire the permissions of scripts generated by the target website and can therefore compromise the confidentiality and integrity of data transfers between the website and client. Websites are vulnerable if they display user-supplied data from requests or forms without sanitizing the data so that it is not executable. -->

#### Cryptographic Authenticator

Cryptographic Key をシークレットとする Authenticator.

<!-- An authenticator where the secret is a cryptographic key. -->

#### Cryptographic Key

復号, 暗号化, 署名生成, 署名検証等の暗号論的オペレーションを管理するために用いられる値. 本ガイドライン群では, NIST SP 800-57 Part 1 の Table 2 で述べられた最低限の要件を満たすものとする.

<!-- A value used to control cryptographic operations, such as decryption, encryption, signature generation, or signature verification. For the purposes of these guidelines, key requirements shall meet the minimum requirements stated in Table 2 of NIST SP 800-57 Part 1. -->

Asymmetric Keys および Symmetric Key も参照のこと.

<!-- See also Asymmetric Keys, Symmetric Key. -->

#### Cryptographic Module

一式のハードウェア, ソフトウェア, およびファームウェアで, (暗号アルゴリズムや鍵生成を含む) 承認されたセキュリティ機能を実装するもの.

<!-- A set of hardware, software, and/or firmware that implements approved security functions (including cryptographic algorithms and key generation). -->

#### Data Integrity

Authorize されていないエンティティによってデータが変更されることがないというプロパティー.

<!-- The property that data has not been altered by an unauthorized entity. -->

#### Derived Credential

事前に発行済の Credential に紐づいた Authenticator を所有・管理していることを証明することで発行される Credential. このような Credential を発行することで, 再び Identity Proofing プロセスを繰り返す必要がなくなる.

<!-- A credential issued based on proof of possession and control of an authenticator associated with a previously issued credential, so as not to duplicate the identity proofing process. -->

#### Digital Authentication

情報システムに対して, 電子的に表現されたユーザーの Identity の確からしさを確立するプロセス.
SP 800-63 の前エディションまでは *Electronic Authentication* と呼ばれていたもの.

<!-- The process of establishing confidence in user identities presented digitally to a system. In previous editions of SP 800-63, this was referred to as *Electronic Authentication*. -->

#### Digital Signature

Private Key を用いてデータにデジタル署名を行い, Public Key を用いて署名検証を行う, Asymmetric Key を用いたオペレーション. Digital Signature は Authenticity Protection (真正性保護), Integrity Protection (完全性保護), Non-repudiation (否認防止) を提供するが, Confidentiality Protection (機密性保護) は提供しない.

<!-- An asymmetric key operation where the private key is used to digitally sign data and the public key is used to verify the signature. Digital signatures provide authenticity protection, integrity protection, and non-repudiation, but not confidentiality protection.   -->

#### Diversionary

KBV において, 多項選択式問題の全選択肢がまちがいであり, Applicant に "none of the above" といった選択肢を選択するよう要求するもの.

<!-- In regards to KBV, a multiple-choice question for which all answers provided are incorrect, requiring the applicant to select an option similar to "none of the above." -->

#### Eavesdropping Attack

Authentication Protocol を受動的に傾聴し, 情報を傍受する Attack. 傍受した情報は, 後続の Claimant に成りすました Active Attack で利用される.

<!-- An attack in which an attacker listens passively to the authentication protocol to capture information that can be used in a subsequent active attack to masquerade as the claimant. -->

#### Electronic Authentication (E-Authentication)

*Digital Authentication* 参照.

<!-- See *Digital Authentication*. -->

#### <a name="enroll"></a> Enrollment

Applicant が CSP の Subscriber となるべく申し込み, CSP が Applicant の Identity を検証するプロセス.

<!-- The process through which an applicant applies to become a subscriber of a CSP and the CSP validates the applicant's identity. -->

#### Entropy

Attacker がシークレット値を決定することに対峙する際の不確実性の量の尺度. Entropy は通常ビットで表現される. *n* ビットの Entropy を持つ値は,  *n* ビットの一様乱数と同等の不確実性を持つ.

<!-- A measure of the amount of uncertainty an attacker faces to determine the value of a secret. Entropy is usually stated in bits. A value having *n* bits of entropy has the same degree of uncertainty as a uniformly distributed *n*-bit random value. -->

#### Federal Information Processing Standard (FIPS)

Secretary of Commerce は, Information Technology Management Reform Act (Public Law 104-106) に基づいて, National Institute of Standards and Technology (NIST) により連邦政府機関のコンピュータシステムに適用するために開発された標準及びガイドラインを承認する. これらの標準及びガイドラインは NIST によって FIPS として発行されたものであり, 政府機関で横断的に使われるものである. NIST はセキュリティや相互運用性といった強制力のある連邦政府の要求事項がある場合や, 許容可能な業界標準やソリューションが存在しない場合に, FIPS を開発する. 詳細については背景を参照すること.

<!-- Under the Information Technology Management Reform Act (Public Law 104-106), the Secretary of Commerce approves the standards and guidelines that the National Institute of Standards and Technology (NIST) develops for federal computer systems. NIST issues these standards and guidelines as Federal Information Processing Standards (FIPS) for government-wide use. NIST develops FIPS when there are compelling federal government requirements, such as for security and interoperability, and there are no acceptable industry standards or solutions. See background information for more details. -->

FIPS ドキュメントは FIPS ホームページ <http://www.nist.gov/itl/fips.cfm> からオンラインアクセス可能である.

<!-- FIPS documents are available online on the FIPS home page: <http://www.nist.gov/itl/fips.cfm> -->

#### Federation

一連のネットワークシステム間で Identity および Authentication 情報の伝搬を行うためのプロセス.

<!-- A process that allows the conveyance of identity and authentication information across a set of networked systems. -->

#### Federation Assurance Level (FAL)

Federation において Authentication 情報および (場合によっては) Attribute 情報を RP に送る際に用いられる Assertion Protocol のカテゴリー.

<!-- A category describing the assertion protocol used by the federation to communicate authentication and attribute information (if applicable) to an RP. -->

#### Federation Proxy

IdP に対して論理的に RP として動作し, RP に対して論理的に IdP として動作する, 2つのシステムを Bridge するコンポーネント. "Broker" と呼ばれることもある.

<!-- A component that acts as a logical RP to a set of IdPs and a logical IdP to a set of RPs, bridging the two systems with a single component. These are sometimes referred to as "brokers". -->

#### Front-Channel Communication

ブラウザ等を媒介とし, 2つのシステム間でリダイレクトを用いて行われるコミュニケーション.
これは通常メッセージ受信者がホストする URL に HTTP Query Parameter を付与することで実現される.

<!-- Communication between two systems that relies on redirects through an intermediary such as a browser. This is normally accomplished by appending HTTP query parameters to URLs hosted by the receiver of the message. -->

#### Hash Function

任意長の短いの文字列を固定長の文字列に変換する関数. 承認されている Hash Function は以下のプロパティーを満たす.

<!-- A function that maps a bit string of arbitrary length to a fixed-length bit string. Approved hash functions satisfy the following properties: -->

1. 一方向性 - 指定された出力結果から対応する入力を特定することが計算上困難であり

<!-- 1. One-way - It is computationally infeasible to find any input that maps to any pre-specified output; and -->

2. 衝突困難性 - 同じ出力となる任意の2つの異なる入力を特定することが計算上困難である.

<!-- 2. Collision resistant - It is computationally infeasible to find any two distinct inputs that map to the same output. -->

#### Identity

特定のコンテキストにおいて, ある Subject を他と区別できるかたちで表現する, Attribute ないしは Attribute の集合.

<!-- An attribute or set of attributes that uniquely describe a subject within a given context. -->

#### Identity Assurance Level (IAL)

Applicant の Claimed Identity が本人の本物の Identity であることの確からしさの度合いをあらわすカテゴリー.

<!-- A category that conveys the degree of confidence that the applicant's claimed identity is their real identity. -->

#### Identity Evidence

Applicant が Claimed Identity を裏付ける為に提出する情報ないしはドキュメント. Identity Evidence は物理的存在 (e.g. 免許証) なこともあればデジタルな存在 (e.g. CSP が Applicant を Authenticate した上で発行した Assertion) なこともある.

<!-- Information or documentation provided by the applicant to support the claimed identity. Identity evidence may be physical (e.g. a driver license) or digital (e.g. an assertion generated and issued by a CSP based on the applicant successfully authenticating to the CSP). -->

#### Identity Proofing

CSP が Credential 発行のためにある人物に関する情報を収集, 確認, および検証するプロセス.

<!-- The process by which a CSP collects, validates, and verifies information about a person. -->

#### Identity Provider (IdP)

Subscriber のプライマリーな Authentication Credentials を管理し, Assertion を発行する主体.
本ドキュメント群においては, CSP とも呼ばれる.

<!-- The party that manages the subscriber’s primary authentication credentials and issues assertions derived from those credentials. This is commonly the CSP as discussed within this document suite. -->

#### Issuing Source

Identity Evidence として利用可能なデータや Assertion などのデジタルなエビデンス, 物理的ドキュメント等を責任を持って生成するオーソリティー.

<!-- An authority responsible for the generation of data, digital evidence (such as assertions), or physical documents that can be used as identity evidence. -->

#### Kerberos

MIT で開発された, 幅広く利用されている Authentication Protocol. "classic" な Kerberos では, ユーザは秘密のパスワードを Key Distribution Center (KDC) に共有する. ユーザ Alice は他のユーザ Bob と通信するため KDC に対して Authenticate し, KDC は "ticket" を発行する. 当該チケットは Alice が Bob に対して Authenticate する為に利用する.

<!-- A widely used authentication protocol developed at MIT. In "classic" Kerberos, users share a secret password with a Key Distribution Center (KDC). The user (Alice) who wishes to communicate with another user (Bob) authenticates to the KDC and the KDC furnishes a "ticket" to use to authenticate with Bob. -->

詳細は [SP 800-63C](sp800-63c.html) Section 11.2 を参照のこと.

<!-- See [SP 800-63C](sp800-63c.html) Section 11.2 for more information. -->

#### Knowledge-Based Verification (KBV)

当該個人の Claimed Identity と関連づけられているプライベートな情報を知っていることを根拠とした Identity 検証方法. Knowledge-Based Authentication (KBA) や Knowledge-Based Proofing (KBP) とも呼ばれる.

<!-- Identity verification method based on knowledge of private information associated with the claimed identity. This is often referred to as knowledge-based authentication (KBA) or knowledge-based proofing (KBP). -->

#### Man-in-the-Middle Attack (MitM)

Attacker が通信を行う2つの主体の間に介在し, 両者の間を行き交うデータを傍受したり改ざんしたりする Attack. Authentication のコンテキストでは Attacker は Claimant と Verifier の間に介在し, Enrollment のコンテキストでは Registrant と CSP の間, Authenticator 紐付けのコンテキストでは Subscriber と CSP の間に介在することとなる.

<!-- An attack in which an attacker is positioned between two communicating parties in order to intercept and/or alter data traveling between them. In the context of authentication, the attacker would be positioned between claimant and verifier, between registrant and CSP during enrollment, or between subscriber and CSP during authenticator binding. -->

#### Memorized Secret

Subscriber に記憶されることを前提とした, 文字列からなる Authenticator. Subscriber が Authentication プロセスにおいて *something they know* を立証するために利用できる.

<!-- A type of authenticator comprised of a character string intended to be memorized or memorable by the subscriber, permitting the subscriber to demonstrate *something they know* as part of an authentication process. -->

#### Message Authentication Code (MAC)

暗号理論に基づくデータのチェックサムであり, Synmmetric Key を用いてデータの予期していない変更及び意図的な変更との両方を検知するために用いられる. MAC は Authenticity (真正性) と Integrity (完全性) の保護を行うが, Non-repudiation (否認防止) は行わない.

<!-- A cryptographic checksum on data that uses a symmetric key to detect both accidental and intentional modifications of the data. MACs provide authenticity and integrity protection, but not non-repudiation protection. -->

#### Mobile Code

実行コードであり, 通常は提供元から別のコンピューターシステムに転送されたのち実行されるもの. 転送は Network を介す (e.g., Web ページに埋め込まれた JavaScript) が, 物理的なメディアを介して転送されることもある.

<!-- Executable code that is normally transferred from its source to another computer system for execution. This transfer is often through the network (e.g., JavaScript embedded in a web page) but may transfer through physical media as well. -->

#### Multi-Factor

2つ以上の [Authentication Factor](#af) を要求する Authentication システムや Authenticator の特徴. MFA には, 2つ以上の要素を提供する単一の Authenticator を用いてもよいし, 互いに異なる要素を提供する複数の Authenticator を組み合わせて用いてもよい.

<!-- A characteristic of an authentication system or an authenticator that requires more than one distinct [authentication factor](#af) for successful authentication. MFA can be performed using a single authenticator that provides more than one factor or by a combination of authenticators that provide different factors. -->

Authentication Factor としては, Something You Know, Something You Have, Something You Are の3種類がある.

<!-- The three authentication factors are something you know, something you have, and something you are. -->

#### <a name="mfa-definition"></a>Multi-Factor Authentication (MFA)

2つ以上の [Authentication Factor](#af) を要求する Authentication システム. Multi-Factor Authentication は単一の Multi-Factor Authenticator を用いて実現してもよいし, 互いに異なる要素を提供する複数の Authenticator を組み合わせて用いてもよい.

<!-- An authentication system that requires more than one distinct [authentication factor](#af) for successful authentication. Multi-factor authentication can be performed using a multi-factor authenticator or by a combination of authenticators that provide different factors. -->

Authentication Factor としては, Something You Know, Something You Have, Something You Are の3種類がある.

<!-- The three authentication factors are *something you know*, *something you have*, and *something you are*. -->

#### Multi-Factor Authenticator

2つ以上の Authentication Factor を提供する Authenticator. デバイスをアクティベートする為の Biometric センサーを持った暗号論的 Authentication デバイスなど.

<!-- An authenticator that provides more than one distinct authentication factor, such as a cryptographic authentication device with an integrated biometric sensor that is required to activate the device. -->

#### Network

オープンなコミュニケーションの伝達手段. Internet がその代表例である. Network は Claimant とその他の主体の間でメッセージを伝達するために用いられる. 特に明示されない限り, Network のセキュリティは前提とされず, オープンであり, 任意の主体 (e.g., Claimant, Verifier, CSP および RP) 間の任意の点において Active Attack (e.g., なりすまし, Man-in-the-Middle, Session Hijacking) や Passive Attack (e.g., 盗聴) にさらされうるものと想定される.

#### Nonce

セキュリティプロトコル中で利用され, 同じキーによる繰り返しを許さない値. 例えば, Nonce は Challenge-Response Authentication Protocol のチャレンジとして利用され, Authentication キーが変更されるまでの間繰り返されないものとする (SHALL NOT). さもなければ Replay Attack の可能性がある. Nonce をチャレンジとして利用することと, チャレンジをランダムにすることとは異なる要件である. Nonce は必ずしも予測不可能である必要性はない.

<!-- A value used in security protocols that is never repeated with the same key. For example, nonces used as challenges in challenge-response authentication protocols SHALL not be repeated until authentication keys are changed. Otherwise, there is a possibility of a replay attack. Using a nonce as a challenge is a different requirement than a random challenge, because a nonce is not necessarily unpredictable. -->

#### Offline Attack

Attacker が何らかの情報を得る (典型的には Authentication Protocol 中のやりとりを盗聴したり, システムに侵入してセキュリティファイルを盗む) ことで, 対象となるシステムを解析する Attack.

<!-- An attack where the attacker obtains some data (typically by eavesdropping on an authentication protocol run or by penetrating a system and stealing security files) that he/she is able to analyze in a system of his/her own choosing. -->

#### Online Attack

Authentication Protocol において, 正当な Verifier に対して Claimant のふりをしたり, または能動的に Authentication チャネルを改ざんする Attack.

<!-- An attack against an authentication protocol where the attacker either assumes the role of a claimant with a genuine verifier or actively alters the authentication channel. -->

#### Online Guessing Attack

Attacker が Authenticator Output の取りうる値を推測してログオン試行を繰り返す Attack.

<!-- An attack in which an attacker performs repeated logon trials by guessing possible values of the authenticator output. -->

#### Pairwise Pseudonymous Identifier

CSP が特定の RP に対して生成する, opaque で推測不可能な Subscriber の識別子. この識別子は特定の CSP-RP ペア以外には知られることはなく, 当該ペア間でのみ用いられる.

<!-- An opaque unguessable subscriber identifier generated by a CSP for use at a specific individual RP. This identifier is only known to and only used by one CSP-RP pair. -->

#### Passive Attack

Authentication Protocol に対する Attack. Claimant と Verifier との間を Network を介してやり取りされるデータを傍受するが, データは改ざんしない (例. 盗聴).

<!-- An attack against an authentication protocol where the attacker intercepts data traveling along the network between the claimant and verifier, but does not alter the data (i.e., eavesdropping). -->

#### Passphrase

Passphrase は Claimant が自身の Identity を Authenticate する際に利用する, 単語列や文字列からなる Memoized Secret. Passphrase は Password と似ているが, 一般的には Password より長くセキュリティー的にも強固である.

<!-- A passphrase is a memorized secret consisting of a sequence of words or other text that a claimant uses to authenticate their identity. A passphrase is similar to a password in usage, but is generally longer for added security. -->

#### Password

*Memoized Secret* 参照.

<!-- See *memorized secret*. -->

#### Personal Data

*Personally Identifiable Information* 参照.

<!-- See *Personally Identifiable Information*. -->

#### Personal Identification Number (PIN)

通常10進数の数値のみで構成された Memoized Secret.

<!-- A memorized secret typically consisting of only decimal digits. -->

#### Personal Information

*Personally Identifiable Information* 参照.

<!-- See *Personally Identifiable Information*. -->

#### Personally Identifiable Information (PII)

[OMB Circular A-130](#A-130) で定義されているように, Personally Identifiable Information とは個人の Identity を識別したり追跡するために用いられる情報である. 単体の情報で個人を識別・追跡可能なものもあれば, 特定の個人に紐付け済もしくは紐付け可能なその他の情報と組み合わせることで識別・追跡可能となるものもある.

<!-- As defined by [OMB Circular A-130](#A-130), Personally Identifiable Information is information that can be used to distinguish or trace an individual's identity, either alone or when combined with other information that is linked or linkable to a specific individual. -->

#### Pharming

DNS (Domain Name System) のようなインフラストラクチャサービスを汚染する手法により, Subscriber を偽の Verifier/RP へ誘導し, 機微情報を入力させる, 有害なソフトウェアをダウンロードさせる, 詐欺活動に加担させるような Attack.

<!-- An attack in which an attacker corrupts an infrastructure service such as DNS (Domain Name System) causing the subscriber to be misdirected to a forged verifier/RP, which could cause the subscriber to reveal sensitive information, download harmful software, or contribute to a fraudulent act. -->

#### Phishing

Subscriber を (主に Email を通じて) 偽の Verifier/RP に誘導し, 本物の Verifier/RP に対して Subscriber になりすますための情報を騙し取る Attack.

<!-- An attack in which the subscriber is lured (usually through an email) to interact with a counterfeit verifier/RP and tricked into revealing information that can be used to masquerade as that subscriber to the real verifier/RP. -->

#### Possession and Control of an Authenticator

Authenticator Protocol において, Authenticator をアクティベートし利用する能力.

<!-- The ability to activate and use the authenticator in an authentication protocol. -->

#### Practice Statement

Authentication プロセスの当事者 (e.g., CSP, Verifier) が従う実践的な内容を正式に記載した文書. 通常, 当事者のポリシーと実行内容が記述されており, 法的拘束力を持つ可能性がある.

<!-- A formal statement of the practices followed by the parties to an authentication process (e.g., CSP or verifier). It usually describes the parties' policies and practices and can become legally binding. -->

#### Private Credentials

Authenticator へのセキュリティー侵害につながるため, CSP によって開示されることがない Credential.

<!-- Credentials that cannot be disclosed by the CSP because the contents can be used to compromise the authenticator. -->

#### Private Key

Asymmetric Key ペアの秘密鍵. データへのデジタル署名や復号に用いられる.

<!-- The secret part of an asymmetric key pair that is used to digitally sign or decrypt data. -->

#### Presentation Attack

Biometric システムの運用妨害を目的とした Biometric データ読み取りサブシステムへの提示.

<!-- Presentation to the biometric data capture subsystem with the goal of interfering with the operation of the biometric system. -->

#### Presentation Attack Detection (PAD)

Presentation Attack の自動検知. Presentation Attack Detection 手法のサブセットである *liveness detection* では, 解剖学的特徴または非自発的または自発的反応の測定および分析を行い, Biometric サンプルが生体の Subject から直接読み取られたものかどうかを判定する.

<!-- Automated determination of a presentation attack. A subset of presentation attack determination methods, referred to as *liveness detection*, involve measurement and analysis of anatomical characteristics or involuntary or voluntary reactions, in order to determine if a biometric sample is being captured from a living subject present at the point of capture. -->

#### Protected Session

2者間でやりとりされるメッセージを, Session Key と呼ばれる Shared Secret を用いて暗号化し, Integrity を保護する Session.

<!-- A session wherein messages between two participants are encrypted and integrity is protected using a set of shared secrets called session keys. -->

当該 Session 内で, ある主体が Session Key に加えて1つ以上の Authenticator を所有していることを証明し, もう一方の主体が当該 Authenticator に紐づく Identity を検証できる場合, 当該主体は *Authenticated* であると言う. もし両主体が共に Authenticated となる場合, この Protected Session は *Mutually Authenticated* であると言える.

<!-- A participant is said to be *authenticated* if, during the session, they prove possession of one or more authenticators in addition to the session keys, and if the other party can verify the identity associated with the authenticator(s). If both participants are authenticated, the protected session is said to be *mutually authenticated*. -->

#### Protected Session

Authenticate され保護されたチャネルで確立された Session.

<!-- A session established on an authenticated protected channel. -->

#### Pseudonym

実名 (Legal Name) 以外の名前.

<!-- A name other than a legal name. -->

#### Pseudonymity

Subject の識別に Pseudonym を用いること.

<!-- The use of a pseudonym to identify a subject. -->

#### Pseudonymous Identifier

RP による Subscriber に関するいかなる推測をも許さず, かつ RP が複数のインタラクションに渡って Subscriber の Claimed Identity を紐づけられるような, 意味のないユニークな識別子.

<!-- A meaningless but unique number that does not allow the RP to infer anything regarding the subscriber but which does permit the RP to associate multiple interactions with the subscriber's claimed identity. -->

#### Public Credentials

セキュリティー侵害を伴わず Authenticator との紐付けを表せる Credential.

<!-- Credentials that describe the binding in a way that does not compromise the authenticator. -->

#### Public Key

Asymmetric Key ペアの公開鍵. データへの署名検証や暗号化に用いられる.

<!-- The public part of an asymmetric key pair that is used to verify signatures or encrypt data. -->

#### Public Key Certificate

Certificate Authority によって発行され, Certificate Authority の秘密鍵でデジタル署名された電子文書. Public Key Certificate により Subscriber の識別子が Public Key と紐づけられる. 当該 Certificate により識別される Subscriber のみが Private Key の管理および Access を持っていることが暗示される. [[RFC 5280]](sp800-63b.ja.html#RFC5280) も参照のこと.

<!-- A digital document issued and digitally signed by the private key of a certificate authority that binds an identifier to a subscriber to a public key. The certificate indicates that the subscriber identified in the certificate has sole control and access to the private key. See also [[RFC 5280]](sp800-63b.html#RFC5280). -->

#### Public Key Infrastructure (PKI)

Certificate と Public-Private Key Pair を管理する目的で利用される, 一連のポリシー, プロセス, サーバープラットフォーム, ソフトウェア, ワークステーションなど. Public Key Certificate の発行, 管理, 失効を行う能力を備える.

<!-- A set of policies, processes, server platforms, software, and workstations used for the purpose of administering certificates and public-private key pairs, including the ability to issue, maintain, and revoke public key certificates. -->

#### Reauthentication

ある Session において, Subscriber が継続してその場に存在し Authenticate する意思を持っていることを確認するプロセス.

<!-- The process of confirming the subscriber's continued presence and intent to be authenticated during an extended usage session. -->

#### Registration

[Enrollment](#enroll) 参照.

<!-- See [Enrollment](#enroll). -->

#### Relying Party (RP)

Subscriber の Authenticator および Credential, Verifier の Claimant Identity に関する Assertion を信頼して, Transaction を処理したり情報やシステムへの Access を許可したりする主体.

<!-- An entity that relies upon the subscriber's authenticator(s) and credentials or a verifier's assertion of a claimant's identity, typically to process a transaction or grant access to information or a system. -->

#### Remote

(*Remote Authentication や Remote Transaction といったコンテキストで*) 単一組織によるセキュリティ対策のみでは End-to-End での確実な保護が期待できないような状況下での, Network 接続されたデバイス間の情報交換.

<!-- (*In the context of remote authentication or remote transaction*) An information exchange between network-connected devices where the information cannot be reliably protected end-to-end by a single organization's security controls. -->

#### Replay Attack

Attacker が事前に記録しておいた (正当な Claimant と Verifier との間の) メッセージを, Verifier に対して Claimant になりすまして, もしくはその逆方向に, 再送する Attack.

<!-- An attack in which the attacker is able to replay previously captured messages (between a legitimate claimant and a verifier) to masquerade as that claimant to the verifier or vice versa. -->

#### Replay Resistance

Replay Attack 耐性のある Authentication プロセスのプロパティ. 典型的には, 特定の Authentication にのみ有効な Authenticator Output により実現される.

<!-- The property of an authentication process to resist replay attacks, typically by use of an authenticator output that is valid only for a specific authentication. -->

#### Restricted

誤認発生時に追加のリスクが発生するため, 追加要件を要求されるような Authenticator Type, クラス, インスタンス.

<!-- An authenticator type, class, or instantiation having additional risk of false acceptance associated with its use that is therefore subject to additional requirements. -->

#### Risk Assessment

システムの運用に起因する, 組織の運営 (ミッション, 機能, イメージ, レピュテーションを含む), 組織の資産, 個人および他組織に対するリスクを, 特定, 推定, 優先順位付けするプロセス. Risk Assessment は Risk Management の一部であり, 脅威・脆弱性解析を包含し, 計画済もしくは実施中のセキュリティー管理で施される対策について考察するプロセスである. Risk Analysis と同義.

<!-- The process of identifying, estimating, and prioritizing risks to organizational operations (including mission, functions, image, or reputation), organizational assets, individuals, and other organizations, resulting from the operation of a system. It is part of risk management, incorporates threat and vulnerability analyses, and considers mitigations provided by security controls planned or in place. Synonymous with risk analysis. -->

#### Risk Management

組織の運営 (ミッション, 機能, イメージ, レピュテーションを含む), 組織の資産, 個人および他組織に対する情報セキュリティーリスクを管理するプログラムや補助的プロセス. (i) リスクに関係するアクティビティーを見極め, (ii) リスクを評価し, (iii) ひとたびリスクが特定されればそれに対応し, (iv) 継続的にリスクをモニタリングする, という一連の行動を含む.

<!-- The program and supporting processes to manage information security risk to organizational operations (including mission, functions, image, reputation), organizational assets, individuals, other organizations, and includes: (i) establishing the context for risk-related activities; (ii) assessing risk; (iii) responding to risk once determined; and (iv) monitoring risk over time. -->

#### Salt

暗号プロセスにおいて用いられる秘密でない (non-secret) 値で, 通常ある特定の計算結果が Attacker によって再利用されないことを保証するために用いられる.

<!-- A non-secret value used in a cryptographic process, usually to ensure that the results of computations for one instance cannot be reused by an attacker. -->

#### Secure Sockets Layer (SSL)

*Transport Layer Security (TLS)* 参照.

<!-- See *Transport Layer Security (TLS)*. -->

#### Session

Subscriber と RP ないしは CSP のエンドポイントの間の継続的インタラクション. Session は Authentication イベントにより開始され, Session 終了イベントで終了する. Session は Session シークレットにより紐づけられる. Session シークレットは Subscriber のソフトウェア (Browser, アプリケーション, OS) が RP や CSP に Subscriber の Authentication Credential の代わりとして提示することができる値である.

<!-- A persistent interaction between a subscriber and an endpoint, either an RP or a CSP. A session begins with an authentication event and ends with a session termination event. A session is bound by use of a session secret that the subscriber's software (a browser, application, or OS) can present to the RP or CSP in lieu of the subscriber's authentication credentials. -->

#### Session Hijack Attack

Claimant と Verifier の間の Authentication のやりとりが成功したのち, Attacker が Claimant と Verifier の間に入り込む攻撃. Attacker は Verifier に対して Subscriber のふりをしたり, Subscriber に対して Verifier のふりをしたりして, Session 中のデータ交換をコントロールする. Claimant と RP の間の Session も同様に侵害されうる.

<!-- An attack in which the attacker is able to insert himself or herself between a claimant and a verifier subsequent to a successful authentication exchange between the latter two parties. The attacker is able to pose as a subscriber to the verifier or vice versa to control session data exchange. Sessions between the claimant and the RP can be similarly compromised. -->

#### Shared Secret

Subscriber と Verifier が知っている, Authentication で使われるシークレット.

<!-- A secret used in authentication that is known to the subscriber and the verifier. -->

#### Side-Channel Attack

物理的な暗号システムからの情報漏洩によって可能となる Attack. Side-Channel Attack においては, タイミング, 消費電力, 電磁波放出, 及び音響放射といった特性が利用される.

<!-- An attack enabled by leakage of information from a physical cryptosystem. Characteristics that could be exploited in a side-channel attack include timing, power consumption, and electromagnetic and acoustic emissions. -->

#### <a name="sf"></a> Single-Factor

単一の Authentication Factor (something you know, something you have, or something you are) を要求する Authentication システムや Authenticator の特徴.

<!-- A characteristic of an authentication system or an authenticator that requires only one authentication factor (something you know, something you have, or something you are) for successful authentication. -->

#### Social Engineering

人を欺いてセンシティブな情報を露呈させたり, Unauthorized Access を得たり, 人と付き合いながら信頼を獲得した上で詐欺を行う活動.

<!-- The act of deceiving an individual into revealing sensitive information, obtaining unauthorized access, or committing fraud by associating with the individual to gain confidence and trust. -->

#### Special Publication (SP)

NIST が発行する出版物の一形態. 特に SP 800 シリーズは, Information Technology Laboratory による研究活動, ガイドライン, コンピュターセキュリティ分野における公共福祉のための支援活動, 民間・政府・学術組織との協調的な活動などに関するレポートとなっている.

<!-- A type of publication issued by NIST. Specifically, the SP 800-series reports on the Information Technology Laboratory's research, guidelines, and outreach efforts in computer security, and its collaborative activities with industry, government, and academic organizations. -->

#### Subject

人間, 組織, デバイス, ハードウェア, ネットワーク, ソフトウェア, サービスなど.

<!-- A person, organization, device, hardware, network, software, or service. -->

#### Subscriber

CSP から Credential や Authenticator を受け取る主体.

<!-- A party who has received a credential or authenticator from a CSP. -->

#### Symmetric Key

暗号化と復号, Message Authentication Code の生成と検証などの, 対となる暗号論的オペレーションの両方で用いられる暗号論的な鍵.

<!-- A cryptographic key used to perform both the cryptographic operation and its inverse. For example, to encrypt and decrypt or create a message authentication code and to verify the code. -->

#### Token

[Authenticator](#authenticator) 参照.

<!-- See [Authenticator](#authenticator). -->

#### Token Authenticator

[Authenticator Output](#authenticatoroutput) 参照.

<!-- See [Authenticator Output](#authenticatoroutput). -->

#### Token Secret

[Authenticator Secret](#authenticatorsecret) 参照.

<!-- See [Authenticator Secret](#authenticatorsecret). -->

#### Transaction

ビジネスやプログラムの目標を支援する, ユーザーとシステムの間の個別のイベント. 政府のデジタルシステムにおいては, デジタル Identity の Risk Assessment において個別の分析が必要な, 複数のカテゴリーないしはタイプの Transaction がある.

<!-- A discrete event between a user and a system that supports a business or programmatic purpose. A government digital system may have multiple categories or types of transactions, which may require separate analysis within the overall digital identity risk assessment. -->

#### Transport Layer Security (TLS)

ブラウザやウェブサーバに広く実装されている Authentication およびセキュリティプロトコル. TLSは [[RFC 5246]](#RFC5246) で定義されている. TLSはより古い SSL プロトコルと似ており, 実質的に TLS1.0 は SSL version 3.1 といえる. [[SP 800-52]](#SP800-52) (Guidelines for the Selection and Use of Transport Layer Security (TLS) Implementations) では, 政府のアプリケーションにおいてどのように TLS を用いるかを定めている.

<!-- An authentication and security protocol widely implemented in browsers and web servers. TLS is defined by RFC 5246. TLS is similar to the older SSL protocol, and TLS 1.0 is effectively SSL version 3.1. NIST SP 800-52, Guidelines for the Selection and Use of Transport Layer Security (TLS) Implementations [[SP 800-52]](#SP800-52), specifies how TLS is to be used in government applications. -->

#### Trust Anchor

ハードウェアやソフトウェエアに直接埋め込まれていたり, Out-of-band な手法によりセキュアに提供されたりすることで Trust される, Public Key もしくは Symmetric Key. 他の信頼できる主体の保証を受けて Trust を得るものは Trust Anchor とは呼ばない (Public Key Certificate 等). Trust Anchor はそのスコープを制限するような名称やポリシー制約を持つこともある.

<!-- A public or symmetric key that is trusted because it is directly built into hardware or software, or securely provisioned via out-of-band means, rather than because it is vouched for by another trusted entity (e.g. in a public key certificate). A trust anchor may have name or policy constraints limiting its scope. -->

#### Usability

[ISO/IEC 9241-11](#ISO9241) によると, 特定のユーザが特定の利用コンテキストにおいて, 効果的, 効率的かつ十分に特定の目的を果たすためにプロダクトを利用しうる度合い.

<!-- Per [ISO/IEC 9241-11](#ISO9241): Extent to which a product can be used by specified users to achieve specified goals with effectiveness, efficiency, and satisfaction in a specified context of use. -->

#### Verifier

Authentication Protocol を利用して, Claimant が1つまたは2つの Authenticator を 所有, 管理していることを検証し, Claimant の Identity を検証する主体. Verifier は上記の目的のため, Authenticator と Identity を紐付ける Credential を確認し, そのステータスをチェックする必要がある場合もある.

<!-- An entity that verifies the claimant's identity by verifying the claimant's possession and control of one or two authenticators using an authentication protocol. To do this, the verifier may also need to validate credentials that link the authenticator(s) to the subscriber's identifier and check their status. -->

#### Verifier Impersonation

通常は正規の Verifier に対して Subscriber になりすますために使える情報を詐取することを目的とし, Authentication Protocol において Attacker が Verifier になりすますようなシナリオ. 以前の SP 800-63 では Verifier Impersonation 耐性を持つ Authentication Protocol は "strongly MitM resistant" であると記述されていた.


<!-- A scenario where the attacker impersonates the verifier in an authentication protocol, usually to capture information that can be used to masquerade as a subscriber to the real verifier. In previous editions of SP 800-63, authentication protocols that are resistant to verifier impersonation have been described as "strongly MitM resistant". -->

#### Virtual In-Person Proofing

Remote の Identity Proofing プロセスのうち, 物理的・技術的・手続き上の手段により Remote Session であっても物理的に対面 (In-person) の Identity Proofing プロセスと同等であると考えられるもの.

<!-- A remote identity proofing process that employs physical, technical and procedural measures that provide sufficient confidence that the remote session can be considered equivalent to a physical, in-person identity proofing process. -->

#### Weakly Bound Credentials

Credential を無効化することなく変更可能な方法で, Subscriber と結びつけられた Credentials.

<!-- Credentials that are bound to a subscriber in a manner than can be modified without invalidating the credential. -->

#### Zeroize

データを破壊し復元できないようにするために, ゼロ値のビットだけで構成されるデータによってメモリを上書きすること. これはしばしばデータそのものを破壊するのではなく, ファイルシステム上のデータへの参照を破壊するだけの削除手法と対比される.

<!-- Overwrite a memory location with data consisting entirely of bits with the value zero so that the data is destroyed and not recoverable. This is often contrasted with deletion methods that merely destroy reference to data within a file system rather than the data itself. -->

#### Zero-Knowledge Password Protocol

Claimant が Verifier に対して Authenticate を行う際, Verifier に対して Password を提示する必要がないような Password ベースの Authentication Protocol. 例としては EKE, SPEKE, SRP などが挙げられる.

### A.2 Abbreviations
本ガイドライン群で選択されている略語を以下に定義する.

|Abbreviation|Term|
|:-----|:---------|
|ABAC|Attribute Based Access Control|
|AAL|Authenticator Assurance Level|
|AS|Authentication Server|
|CAPTCHA|Completely Automated Public Turing test to tell Computer and Humans Apart|
|CSP|Credential Service Provider|
|CSRF|Cross-site Request Forgery|
|XSS|Cross-site Scripting|
|DNS|Domain Name System|
|EO|Executive Order|
|FACT Act|Fair and Accurate Credit Transaction Act of 2003|
|FAL|Federation Assurance Level|
|FEDRAMP|Federal Risk and Authorization Management Program|
|FMR|False Match Rate|
|FNMR|False Non-Match Rate|
|FIPS|Federal Information Processing Standard|
|FISMA|Federal Information Security Modernization Act|
|IAL|Identity Assurance Level|
|IM|Identity Manager|
|IdP|Identity Provider|
|IoT|Internet of Things|
|ISO/IEC|International Organization for Standardization/International Electrotechnical Commission|
|JOSE|JSON Object Signing and Encryption|
|JSON|JavaScript Object Notation |
|JWT|JSON Web Token|
|KBA|Knowledge-Based Authentication|
|KBV|Knowledge-Based Verification|
|KDC|Key Distribution Center|
|LOA|Level of Assurance|
|MAC|Message Authentication Code|
|MitM|Man-in-the-Middle|
|MitMA|Man-in-the-Middle Attack|
|MFA|Multi-Factor Authentication|
|N/A|Not Applicable|
|NARA|National Archives and Records Administration|
|OMB|Office of Management and Budget|
|OTP|One-Time Password|
|PAD|Presentation Attack Detection|
|PHI|Personal Health Information|
|PIA|Privacy Impact Assessment |
|PII|Personally Identifiable Information|
|PIN|Personal Identification Number|
|PKI|Public Key Infrastructure |
|PL|Public Law|
|PSTN|Public Switched Telephone Network|
|RA|Registration Authority|
|RMF|Risk Management Framework|
|RP|Relying Party|
|SA&A|Security Authorization & Accreditation|
|SAML|Security Assertion Markup Language|
|SAOP|Senior Agency Official for Privacy|
|SSL|Secure Sockets Layer|
|SMS|Short Message Service|
|SP|Special Publication|
|SORN|System of Records Notice|
|TEE|Trusted Execution Environment|
|TGS|Ticket Granting Server|
|TGT|Ticket Granting Ticket|
|TLS|Transport Layer Security|
|TPM|Trusted Platform Module|
|VOIP|Voice-over-IP|
