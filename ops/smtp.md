# Iyubake ubutumwa bwawe bwa SMTP bwohereza seriveri

## ijambo ry'ibanze

SMTP irashobora kugura serivisi kubacuruzi b'igicu, nka:

* [Amazone SES SMTP](https://docs.aws.amazon.com/ses/latest/dg/send-email-smtp.html)
* [Ali igicu imeri gusunika](https://www.alibabacloud.com/help/directmail/latest/three-mail-sending-methods)

Urashobora kandi kwiyubakira seriveri yawe bwite - kohereza imipaka itagira imipaka, igiciro gito muri rusange.

Hasi, twerekana intambwe ku yindi uburyo twakubaka seriveri yacu bwite.

## Guhitamo Seriveri

Seriveri yonyine ya SMTP isaba IP rusange ifite ibyambu 25, 456, na 587 bifunguye.

Ibicu rusange bikoreshwa byahagaritse ibyo byambu muburyo budasanzwe, kandi birashoboka ko wabifungura utanga itegeko ryakazi, ariko birababaje cyane nyuma ya byose.

Ndasaba kugura kubakira ifite ibyo byambu bifunguye kandi bigashyigikira gushiraho amazina yinyuma.

Hano, ndasaba [Contabo](https://contabo.com) .

Contabo ni umushyitsi utanga icyicaro i Munich, mu Budage, washinzwe mu 2003 hamwe n’ibiciro byapiganwa cyane.

Niba uhisemo Euro nkifaranga ryubuguzi, igiciro kizaba gihendutse (seriveri ifite 8GB yibuka na 4 CPU igura hafi 529 yu mwaka, kandi amafaranga yambere yo kwishyiriraho ni ubuntu kumwaka umwe).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/UoAQkwY.webp)

Mugihe utumije, ijambo `prefer AMD` , na seriveri hamwe na AMD CPU bizagira imikorere myiza.

Mubikurikira, nzafata VPS ya Contabo nkurugero rwo kwerekana uburyo wakubaka seriveri yawe bwite.

## Ubuntu

Sisitemu ikora hano ni Ubuntu 22.04

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/smpIu1F.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/m7Mwjwr.webp)

Niba seriveri kuri ssh yerekana `Welcome to TinyCore 13!` (nkuko bigaragara ku gishushanyo gikurikira), bivuze ko sisitemu itarashyirwaho. Nyamuneka hagarika ssh hanyuma utegereze iminota mike kugirango wongere winjire.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/-qKACz9.webp)

Iyo `Welcome to Ubuntu 22.04.1 LTS` igaragara, intangiriro iruzuye, kandi urashobora gukomeza nintambwe zikurikira.

### [Bihitamo] Gutangiza ibidukikije byiterambere

Iyi ntambwe irahinduka.

Kugirango byorohe, nshyizeho installation na sisitemu ya sisitemu ya ubuntu muri [github.com/wactax/ops.os/tree/main/ubuntu](https://github.com/wactax/ops.os/tree/main/ubuntu) .

Koresha itegeko rikurikira kugirango ushyireho kanda imwe.

```
bash <(curl -s https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

Abakoresha abashinwa, nyamuneka koresha itegeko rikurikira aho, kandi ururimi, umwanya wigihe, nibindi bizahita bishyirwaho.

```
CN=1 bash <(curl -s https://ghproxy.com/https://raw.githubusercontent.com/wactax/ops.os/main/ubuntu/boot.sh)
```

### Contabo ituma IPV6

Gushoboza IPV6 kugirango SMTP nayo yohereze imeri hamwe na aderesi ya IPV6.

Hindura `/etc/sysctl.conf`

Hindura cyangwa wongere imirongo ikurikira

```
net.ipv6.conf.all.disable_ipv6 = 0
net.ipv6.conf.default.disable_ipv6 = 0
net.ipv6.conf.lo.disable_ipv6 = 0
```

Kurikirana hamwe na [contabo yigisha: Ongeraho IPv6 ihuza seriveri yawe](https://contabo.com/blog/adding-ipv6-connectivity-to-your-server/)

Hindura `/etc/netplan/01-netcfg.yaml` 01-netcfg.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/5MEi41I.webp)

Hanyuma `netplan apply` kugirango iboneza ryahinduwe ritangire gukurikizwa.

Nyuma yimiterere igenda neza, urashobora gukoresha `curl 6.ipw.cn` kugirango urebe aderesi ya ipv6 yumuyoboro wawe wo hanze.

## Koresha ububiko bwa ops

```
git clone https://github.com/wactax/ops.soft.git
```

## Kora icyemezo cya SSL kubuntu kubwizina ryawe

Kohereza imeri bisaba icyemezo cya SSL cyo gushishoza no gusinya.

Dukoresha [acme.sh](https://github.com/acmesh-official/acme.sh) kugirango tubyare ibyemezo.

acme.sh nigikoresho gifungura ibikoresho byashyizweho umukono ibyemezo,

Injira iboneza ububiko ops.soft, koresha `./ssl.sh` , hanyuma ububiko bwa `conf` buzashyirwaho **mububiko bwo hejuru** .

Shakisha DNS utanga kuva [acme.sh dnsapi](https://github.com/acmesh-official/acme.sh/wiki/dnsapi) , hindura `conf/conf.sh`

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Qjq1C1i.webp)

Noneho koresha `./ssl.sh 123.com` kugirango ubone `123.com` na `*.123.com` ibyemezo byizina rya domaine.

Kwiruka kwambere bizahita byinjiza [acme.sh](https://github.com/acmesh-official/acme.sh) hanyuma wongereho gahunda iteganijwe yo kuvugurura byikora. Urashobora kubona `crontab -l` , hari umurongo nkuyu ukurikira.

```
52 0 * * * "/mnt/www/.acme.sh"/acme.sh --cron --home "/mnt/www/.acme.sh" > /dev/null
```

Inzira yicyemezo cyatanzwe ni ikintu nka `/mnt/www/.acme.sh/123.com_ecc。`

Kuvugurura ibyemezo bizahamagara `conf/reload/123.com.sh` inyandiko, uhindure iyi nyandiko, urashobora kongeramo amategeko nka `nginx -s reload` kugirango uhindure cache cyemezo cyibisabwa bijyanye.

## Kubaka seriveri ya SMTP hamwe na chasquid

[chasquid](https://github.com/albertito/chasquid) nisoko ifunguye seriveri ya SMTP yanditse mururimi rwa Go.

Nkigisimbuza porogaramu ya seriveri ya kera ya porogaramu nka Postfix na Sendmail, chasquid iroroshye kandi yoroshye kuyikoresha, kandi biroroshye no kwiteza imbere.

Koresha `./chasquid/init.sh 123.com` izashyirwaho mu buryo bwikora ukanze rimwe (gusimbuza 123.com n'izina ryawe ryohereje).

## Shiraho umukono wa imeri DKIM

DKIM ikoreshwa mu kohereza imikono ya imeri kugirango ibuze inyuguti gufatwa nka spam.

Nyuma yuko itegeko rigenda neza, uzasabwa gushyiraho inyandiko ya DKIM (nkuko bigaragara hano).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/LJWGsmI.webp)

Gusa ongeraho inyandiko ya TXT kuri DNS yawe (nkuko bigaragara hano).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/0szKWqV.webp)

## Reba imiterere ya serivisi & ibiti

 `systemctl status chasquid` Reba serivise ya status.

Imiterere yimikorere isanzwe nkuko bigaragara mumashusho hepfo

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/CAwyY4E.webp)

 `grep chasquid /var/log/syslog` cyangwa `journalctl -xeu chasquid` irashobora kureba ikosa ryanditse.

## Hindura imiterere yizina rya domaine

Izina rya domaine ihindagurika ni ukwemerera aderesi ya IP gukemurwa nizina rya domaine ihuye.

Gushiraho izina ryinyuma rishobora kubuza imeri kumenyekana nka spam.

Iyo iposita yakiriwe, seriveri yakiriye izakora isesengura ryizina rya domaine kuri IP aderesi ya seriveri yoherejwe kugirango yemeze niba seriveri yoherejwe ifite izina ryemewe ryemewe.

Niba seriveri yohereje idafite izina rya domaine ihindagurika cyangwa niba izina rya domaine ihindagurika idahuye na IP ya seriveri yoherejwe, seriveri yakira irashobora kumenya imeri nka spam cyangwa ikanga.

Sura [https://my.contabo.com/rdns](https://my.contabo.com/rdns) hanyuma ugene nkuko bigaragara hano hepfo

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/IIPdBk_.webp)

Nyuma yo gushiraho izina rya domaine ihindagurika, ibuka gushiraho imiterere yimbere yizina rya ipv4 na ipv6 kuri seriveri.

## Hindura izina ryakiriwe rya chasquid.conf

Hindura `conf/chasquid/chasquid.conf` ku gaciro k'izina ryinyuma.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/6Fw4SQi.webp)

Noneho kora `systemctl restart chasquid` kugirango utangire serivisi.

## Wibike kuri git ububiko

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/Fier9uv.webp)

Kurugero, Nsubije inyuma ububiko bwa conf kuri progaramu yanjye ya github kuburyo bukurikira

Banza ukore ububiko bwihariye

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ZSCT1t1.webp)

Injira ububiko bwa conf hanyuma utange mububiko

```
git init
git add .
git commit -m "init"
git branch -M main
git remote add origin git@github.com:wac-tax-key/conf.git
git push -u origin main
```

## Ongeraho uwagutumye

kwiruka

```
chasquid-util user-add i@wac.tax
```

Urashobora kongeramo uwagutumye

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/khHjLof.webp)

### Menya neza ko ijambo ryibanga ryashyizweho neza

```
chasquid-util authenticate i@wac.tax --password=xxxxxxx
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/e92JHXq.webp)

Nyuma yo kongeramo umukoresha, `chasquid/domains/wac.tax/users` bazavugururwa, ibuka kubishyikiriza ububiko.

## DNS ongeraho inyandiko ya SPF

SPF (Gahunda yo Kohereza Politiki) ni tekinoroji yo kugenzura imeri ikoreshwa mu gukumira uburiganya bwa imeri.

Igenzura umwirondoro wakohereje amabaruwa mugenzura ko aderesi ya IP yohereje ihuye na DNS inyandiko zizina rya domaine ivuga ko ari, ikabuza abashuka kohereza imeri mbi.

Ongeraho inyandiko za SPF zirashobora kubuza imeri kumenyekana nka spam bishoboka.

Niba seriveri yawe izina rya seriveri idashyigikiye ubwoko bwa SPF, ongeraho ubwoko bwa TXT.

Kurugero, SPF ya `wac.tax` nuburyo bukurikira

`v=spf1 a mx include:_spf.wac.tax include:_spf.google.com ~all`

SPF kuri `_spf.wac.tax`

`v=spf1 a:smtp.wac.tax ~all`

Menya ko `include:_spf.google.com` hano, ibi ni ukubera ko nzashyiraho `i@wac.tax` nka aderesi yoherejwe muri Google mailbox nyuma.

## Ibikoresho bya DNS DMARC

DMARC ni impfunyapfunyo ya (Domisiyo ishingiye ku butumwa bwo kwemeza, gutanga raporo & guhuza).

Byakoreshejwe mu gufata SPF bounces (birashoboka ko byatewe namakosa yimiterere, cyangwa undi muntu witwaza ko ari wowe wohereje spam).

Ongeramo inyandiko ya TXT `_dmarc` ,

Ibirimo ni ibi bikurikira

```
v=DMARC1; p=quarantine; fo=1; ruf=mailto:ruf@wac.tax; rua=mailto:rua@wac.tax
```

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/k44P7O3.webp)

Ibisobanuro bya buri kintu ni nkibi bikurikira

### p (Politiki)

Yerekana uburyo bwo gukora imeri zananirana SPF (Gahunda yo Kohereza Politiki) cyangwa DKIM (DomainKeys Identified Mail) kugenzura. Ibipimo bya p birashobora gushirwa kuri kimwe mubintu bitatu:

* nta na kimwe: Nta gikorwa cyafashwe, gusa ibisubizo byo kugenzura bisubizwa kubohereje binyuze muburyo bwo gutanga imeri.
* Karantine: Shyira iposita itanyuze mu igenzura mu bubiko bwa spam, ariko ntuzanga ubutumwa mu buryo butaziguye.
* kwanga: Kwanga byimazeyo imeri yananiwe kugenzura.

### fo (Amahitamo yo kunanirwa)

Kugaragaza umubare wamakuru yagaruwe nuburyo bwo gutanga raporo. Irashobora gushirwa kuri imwe mu ndangagaciro zikurikira:

* 0: Raporo yo kwemeza ibisubizo kubutumwa bwose
* 1: Gusa menyesha ubutumwa bwananiwe kugenzura
* d: Gusa menyesha izina rya domaine kugenzura kunanirwa
* s: gusa menyesha kunanirwa kugenzura SPF
* l: Gusa menyesha kunanirwa kugenzura DKIM

### rua & ruf

* rua (Raporo URI kuri Raporo Yose): Aderesi imeri yo kwakira raporo zegeranijwe
* ruf (Raporo URI kuri raporo za Forensic): aderesi imeri kugirango wakire raporo zirambuye

## Ongeraho inyandiko za MX kugirango wohereze imeri kuri Google Mail

Kuberako ntabashaga kubona agasanduku k'iposita k'ubuntu gashyigikira aderesi rusange (Catch-All, irashobora kwakira imeri iyo ari yo yose yoherejwe kuri iri zina rya domaine, nta mbogamizi ku mbanzirizamushinga), nakoresheje chasquid yohereza imeri zose kuri posita yanjye ya Gmail.

**Niba ufite agasanduku k'ubucuruzi wishyuye wenyine, nyamuneka ntuhindure MX hanyuma usibe iyi ntambwe.**

Hindura `conf/chasquid/domains/wac.tax/aliases` , shiraho kohereza agasanduku k'iposita

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/OBDl2gw.webp)

`*` yerekana imeri zose, `i` ni imeri ya imeri prefix yumukoresha wohereje hejuru. Kohereza ubutumwa, buri mukoresha agomba kongeramo umurongo.

Noneho ongeraho inyandiko ya MX (nderekeje kuri adresse yizina rya rezo ya domaine hano, nkuko bigaragara kumurongo wambere mumashusho hepfo).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/7__KrU8.webp)

Iboneza rirangiye, urashobora gukoresha izindi aderesi imeri kugirango wohereze imeri kuri `i@wac.tax` `any123@wac.tax` kugirango urebe niba ushobora kwakira imeri muri Gmail.

Niba atari byo, reba logi ya chasquid ( `grep chasquid /var/log/syslog` ).

## Ohereza imeri kuri i@wac.tax hamwe na Google Mail

Google Mail imaze kwakira imeri, mubisanzwe nizeye ko nzasubiza hamwe na `i@wac.tax` aho kuba i.wac.tax@gmail.com.

Sura [https://mail.google.com/mail/u/1/#settings/accounts](https://mail.google.com/mail/u/1/#settings/accounts) hanyuma ukande "Ongeraho indi aderesi imeri".

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/PAvyE3C.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/_OgLsPT.webp)

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/XIUf6Dc.webp)

Noneho, andika kode yo kugenzura yakiriwe na imeri yoherejwe.

Hanyuma, irashobora gushirwaho nka aderesi yoherejwe mbere (hamwe nuburyo bwo gusubiza hamwe na aderesi imwe).

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/a95dO60.webp)

Muri ubu buryo, twarangije gushiraho seriveri ya SMTP kandi mugihe kimwe dukoresha Google Mail kugirango wohereze kandi wakire imeri.

## Ohereza imeri yikizamini kugirango urebe niba iboneza ryagenze neza

Injira `ops/chasquid`

Koresha `direnv allow` kwishyiriraho (direnv yashyizwe mubikorwa byabanjirije urufunguzo rumwe rwo gutangiza kandi ikariso yongewe mugikonoshwa)

hanyuma wiruke

```
user=i@wac.tax pass=xxxx to=iuser.link@gmail.com ./sendmail.coffee
```

Ibisobanuro by'ibipimo ni nkibi bikurikira

* umukoresha: izina rya SMTP
* pass: ijambo ryibanga rya SMTP
* Kuri: uwahawe

Urashobora kohereza imeri yikizamini.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/ae1iWyM.webp)

Birasabwa gukoresha Gmail kugirango wakire imeri yikizamini kugirango urebe niba ibishusho byagenze neza.

### TLS ibanga risanzwe

Nkuko bigaragara ku gishushanyo gikurikira, hano hari akazu gato, bivuze ko icyemezo cya SSL cyashobojwe neza.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/SrdbAwh.webp)

Noneho kanda "Erekana imeri yumwimerere"

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/qQQsdxg.webp)

### DKIM

Nkuko bigaragara ku gishushanyo gikurikira, urupapuro rwumwimerere rwa Gmail rwerekana DKIM, bivuze ko iboneza rya DKIM ryatsinze.

![](https://pub-b8db533c86124200a9d799bf3ba88099.r2.dev/2023/03/an6aXK6.webp)

Reba Ibyakiriwe mumutwe wa imeri yumwimerere, urashobora kubona ko aderesi yawe ari IPV6, bivuze ko IPV6 nayo yagizwe neza.
