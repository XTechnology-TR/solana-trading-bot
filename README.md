# Solana Trading Bot (Beta)

Solana Trading Bot, Solana blok zincirinde token alım satımını otomatikleştirmek için tasarlanmış bir yazılım aracıdır.
Kullanıcı tarafından belirlenen önceden tanımlanmış parametrelere ve stratejilere göre işlemleri yürütmek üzere yapılandırılmıştır.

Bot, havuz yakma, darphane terk etme ve diğer faktörler gibi piyasa koşullarını gerçek zamanlı olarak izleyebilir ve bu koşullar karşılandığında işlemleri yürütür.

## Kurulum

Komut dosyasını çalıştırmak için şunları yapmanız gerekir:

- Yeni boş bir Solana cüzdanı oluşturun
- Ona biraz SOL aktarın.
- Biraz SOL'u USDC veya WSOL'a dönüştürün.
- Aşağıda ayarlanan yapılandırmaya bağlı olarak USDC veya WSOL'a ihtiyacınız olacak.
- `.env.copy` dosyasını güncelleyerek komut dosyasını yapılandırın (işiniz bittiğinde dosya adından .copy'yi kaldırın). - Aşağıdaki [Yapılandırma](#yapılandırma) bölümünü kontrol edin
- Bağımlılıkları şunu yazarak yükleyin: `npm install`
- Komut dosyasını şunu yazarak çalıştırın: `npm run start` terminalde

Aşağıdaki çıktıyı görmelisiniz:

![çıktı](readme/output.png)

### Yapılandırma

#### Cüzdan

- `PRIVATE_KEY` - Cüzdanınızın özel anahtarı.

#### Bağlantı

- `RPC_ENDPOINT` - Solana ağıyla etkileşim kurmak için HTTPS RPC uç noktası.
- `RPC_WEBSOCKET_ENDPOINT` - Solana ağından gerçek zamanlı güncellemeler için WebSocket RPC uç noktası.
- `COMMITMENT_LEVEL` - İşlemlerin taahhüt düzeyi (örneğin, en yüksek güvenlik düzeyi için "sonlandırılmış").

#### Bot

- `LOG_LEVEL` - Günlük kaydı seviyesini ayarlayın, örneğin, `info`, `debug`, `trace`, vb.
- `ONE_TOKEN_AT_A_TIME` - Bir seferde bir token satın almayı işlemek için `true` olarak ayarlayın.
- `COMPUTE_UNIT_LIMIT` - Ücretleri hesaplamak için kullanılan hesaplama limiti.
- `COMPUTE_UNIT_PRICE` - Ücretleri hesaplamak için kullanılan hesaplama fiyatı.
- `PRE_LOAD_EXISTING_MARKETS` - Bot başlangıçta tüm mevcut pazarları belleğe yükleyecektir.
- Bu seçenek genel RPC ile kullanılmamalıdır.
- `CACHE_NEW_MARKETS` - Yeni pazarları önbelleğe almak için `true` olarak ayarlayın.
- Bu seçenek genel RPC ile kullanılmamalıdır. - `TRANSACTION_EXECUTOR` - İşlemleri yürütmek için warp altyapısını kullanmak üzere `warp` olarak ayarlayın veya JSON-RPC jito yürütücüsünü kullanmak üzere jito olarak ayarlayın
- Daha fazla ayrıntı için [warp](#warp-transactions-beta) bölümüne bakın
- `CUSTOM_FEE` - Warp veya jito yürütücüleri kullanılıyorsa bu değer `COMPUTE_UNIT_LIMIT` ve `COMPUTE_UNIT_LIMIT` yerine işlem ücretleri için kullanılacaktır
- Minimum değer 0,0001 SOL'dur ancak 0,006 SOL veya üzeri kullanmanızı öneririz
- Bu ücrete ek olarak, minimum solana ağ ücreti uygulanacaktır

#### Satın al

- `QUOTE_MINT` - Hangi havuzların kesileceği, USDC veya WSOL.
- `QUOTE_AMOUNT` - Her yeni token satın almak için kullanılan miktar. - `AUTO_BUY_DELAY` - Bir token satın almadan önceki milisaniye cinsinden gecikme.
- `MAX_BUY_RETRIES` - Bir token satın almak için maksimum yeniden deneme sayısı.
- `BUY_SLIPPAGE` - Kayma %

#### Satış

- `AUTO_SELL` - Token'ların otomatik satışını etkinleştirmek için `true` olarak ayarlayın.
- Satın alınan token'ları manuel olarak satmak istiyorsanız, bu seçeneği devre dışı bırakın.
- `MAX_SELL_RETRIES` - Bir token'ı satmak için maksimum yeniden deneme sayısı.
- `AUTO_SELL_DELAY` - Bir token'ı otomatik olarak satmadan önceki milisaniye cinsinden gecikme.
- `PRICE_CHECK_INTERVAL` - Kar alma ve zarar durdurma koşullarını kontrol etmek için milisaniye cinsinden aralık.
- Kar alma ve zarar durdurmayı devre dışı bırakmak için sıfır olarak ayarlayın. - `PRICE_CHECK_DURATION` - Stop loss/take profit koşullarını beklemek için milisaniye cinsinden süre.
- Kâr veya zarara ulaşmazsanız bot bu süreden sonra otomatik olarak satış yapacaktır.
- Take profit ve stop loss'u devre dışı bırakmak için sıfıra ayarlayın.
- `TAKE_PROFIT` - Kârın alınacağı kâr yüzdesi.
- Take profit, kotasyon darphanesine göre hesaplanır.
- `STOP_LOSS` - Zararı durdurmak için kayıp yüzdesi.
- Stop loss, kotasyon darphanesine göre hesaplanır.
- `SELL_SLIPPAGE` - Kayma %.

#### Snipe listesi

- `USE_SNIPE_LIST` - Yalnızca `snipe-list.txt` dosyasında listelenen token'ları satın almayı etkinleştirmek için `true` olarak ayarlayın.
- Bot başlamadan önce havuz mevcut olmamalıdır.
- Bot başlamadan önce token ticareti yapılabiliyorsa hiçbir şey olmaz. Bot token'ı satın almaz. - `SNIPE_LIST_REFRESH_INTERVAL` - Snipe listesini yenilemek için milisaniye cinsinden aralık.
- Bot çalışırken snipe listesini güncelleyebilirsiniz. Her yenilediğinde yeni değişiklikleri alacaktır.

Not: Snipe listesi kullanıldığında aşağıdaki filtreler devre dışı bırakılacaktır.

#### Filtreler

- `FILTER_CHECK_INTERVAL` - Havuzun filtrelerle eşleşip eşleşmediğini kontrol etmek için milisaniye cinsinden aralık.
- Filtreleri devre dışı bırakmak için sıfıra ayarlayın.
- `FILTER_CHECK_DURATION` - Havuzun filtrelerle eşleşmesini beklemek için milisaniye cinsinden süre.
- Havuz filtreyle eşleşmiyorsa satın alma işlemi gerçekleşmez.
- Filtreleri devre dışı bırakmak için sıfıra ayarlayın.
- `CONSECUTIVE_FILTER_MATCHES` - Havuzun filtrelerle eşleşmesi için üst üste kaç kez ihtiyaç duyulduğu.
- Bu yararlıdır çünkü havuz yakıldığında (ve sertleştiğinde), diğer filtreler aynı davranışı bildirmeyebilir. örn. havuz boyutu hala eski değere sahip olabilir
- `CHECK_IF_MUTABLE` - Yalnızca meta verileri değiştirilebilir değilse token satın almak için `true` olarak ayarlayın.
- `CHECK_IF_SOCIALS` - Yalnızca en az 1 sosyalleri varsa token satın almak için `true` olarak ayarlayın.
- `CHECK_IF_MINT_IS_RENOUNCED` - Yalnızca darphaneleri terk edilirse token satın almak için `true` olarak ayarlayın.
- `CHECK_IF_FREEZABLE` - Token satın almak için `true` olarak ayarlayın.yalnızca dondurulamazlarsa.
- `CHECK_IF_BURNED` - Yalnızca likidite havuzları yakıldığında token satın almak için `true` olarak ayarlayın.
- `MIN_POOL_SIZE` - Bot yalnızca havuz boyutu belirtilen miktardan büyük veya eşitse satın alır.
- Devre dışı bırakmak için `0` değerini ayarlayın.
- `MAX_POOL_SIZE` - Bot yalnızca havuz boyutu belirtilen miktardan küçük veya eşitse satın alır.
- Devre dışı bırakmak için `0` değerini ayarlayın.

## Warp işlemleri (beta)

Çok sayıda başarısız işlem yaşıyorsanız veya işlem performansı çok yavaşsa, işlemleri yürütmek için `warp` kullanmayı deneyebilirsiniz.
Warp, üçüncü taraf sağlayıcılarla entegrasyonlar kullanarak işlemleri yürüten barındırılan bir hizmettir.

İşlemler için warp kullanmak, bu projenin arkasındaki ekibi destekler.

### Güvenlik

Warp kullanıldığında, işlem barındırılan hizmete gönderilir. **Gönderilen yük cüzdanınızın özel anahtarınızı İÇERMEYECEKTİR**. Ücret işlemi makinenizde imzalanır.
Her istek barındırılan hizmet tarafından işlenir ve üçüncü taraf sağlayıcıya gönderilir.
**İşlemlerinizi veya özel anahtarınızı saklamıyoruz.**

Not: Warp işlemleri varsayılan olarak devre dışıdır.

### Ücretler

Warp'ı işlemler için kullanırken, ücret warp geliştiricileri ve üçüncü taraf sağlayıcılar arasında dağıtılır.
TX başarısız olursa, hesabınızdan ücret alınmaz.

## Yaygın sorunlar

Burada listelenmeyen bir hatanız varsa, lütfen bu depoda yeni bir sorun oluşturun.
Bir sorun hakkında daha fazla bilgi toplamak için lütfen `LOG_LEVEL` öğesini `debug` olarak değiştirin.

### Desteklenmeyen RPC düğümü

- Günlük dosyanızda aşağıdaki hatayı görürseniz:
`Hata: 410 Gitti: {"jsonrpc":"2.0","error":{"code": 410, "message":"RPC çağrısı veya parametreleri devre dışı bırakıldı."}, "id": "986f3599-b2b7-47c4-b951-074c19842bad" }`
Bu, RPC düğümünüzün betiği yürütmek için gereken yöntemleri desteklemediği anlamına gelir.
- DÜZELTME: RPC düğümünüzü değiştirin. Helius veya Quicknode kullanabilirsiniz.

### Token hesabı yok

- Günlük dosyanızda aşağıdaki hatayı görürseniz:
`Hata: Cüzdanda SOL token hesabı bulunamadı: `
Bu, sağladığınız cüzdanın USDC/WSOL token hesabına sahip olmadığı anlamına gelir. - DÜZELTME: Dex'e gidin ve biraz SOL'u USDC/WSOL ile değiştirin. Örneğin, sol'u wsol ile değiştirdiğinizde cüzdanda aşağıda gösterildiği gibi görmelisiniz:

![wsol](readme/wsol.png)

## İletişim

[![](https://img.shields.io/discord/1201826085655023616?color=5865F2&logo=Discord&style=flat-square)](https://discord.gg/xYUETCA2aP)

## Sorumluluk Reddi

Solana Trading Bot, öğrenme amaçlı olduğu gibi sağlanır.
Kripto para birimleri ve token ticareti risk içerir ve geçmiş performans gelecekteki sonuçların göstergesi değildir.
Bu botu kullanmak sizin kendi riskinizdir ve botu kullanırken oluşan kayıplardan sorumlu değiliz.
