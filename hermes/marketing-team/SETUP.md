# Cài Marketing Team cho Hermes Agent

Hermes ơi, đây là hướng dẫn dựng **Marketing Team** của Distraction Foundry: 13 role (7 role của Team Social, 2 role của Team SEO là Lelouch và Edward, Zoro là Designer, Vail là Trinh sát đối thủ, Tsunade là Account lead giữ phạm vi, hạn, nghiệm thu khi làm dự án khách và Kakashi là Brain keeper giữ brain, chấm bài đã duyệt, viết learnings, bốn role dùng chung cho mọi team; Quintaro viết cho cả Team Social và Team SEO), mỗi role là một **profile** Hermes riêng (SOUL.md, skill, toolset riêng), mỗi role nói chuyện trong **một topic** của một nhóm Telegram. Đọc hết file này, làm theo từng bước, dừng lại hỏi Ông Chủ ở các bước có ghi **[Ông Chủ]**.

Mọi file nằm dưới `BASE = https://0xdchu.github.io/hermes/marketing-team/`. Danh sách đầy đủ: `BASE/manifest.txt` (mỗi dòng một đường dẫn tương đối).

Tham chiếu tài liệu Hermes: Profiles, Running Many Gateways (multiplexing, `profile_routes`), Telegram (Group Chat Usage), Skills (`hermes skills install <url>`), Context Files (`AGENTS.md`), Toolsets Reference.

## 0. Kiểm trước
- `hermes --version` chạy được và gateway Telegram của profile mặc định đã chạy (đã có bot token, `TELEGRAM_ALLOWED_USERS` gồm Ông Chủ).
- Không ghi đè profile đã có: nếu trùng tên `shikamaru yugi ronin quintaro usop wiz xaiyan zoro vail lelouch edward tsunade kakashi`, dừng và hỏi Ông Chủ. Máy đã cài 9 role trước ngày 2026-11-13 thì làm theo mục "Thêm Team SEO" ở cuối file; máy đã cài 12 role (chưa có `kakashi`) thì làm theo mục "Thêm Kakashi" ở cuối file. Máy còn profile `teppi` thì làm mục "Đổi tên Teppi thành Quintaro" ở cuối file **trước** các mục trên.

## 1. Tải workspace của team
Workspace là thư mục làm việc chung của 13 role: `~/work/0xdonniechu/marketing-team/social/` (thư mục của Team Social; Zoro, Vail và Team SEO làm việc ở đây, Zoro vì cần `formats/` và `voice.md`, Vail vì ghi vào `work/intel/`, Team SEO vì ghi vào `work/seo/` và đọc `voice.md`, `work/intel/`; Kakashi vì ghi vào `brain/` và `work/learnings/`).
```bash
BASE=https://0xdchu.github.io/hermes/marketing-team
mkdir -p ~/work/0xdonniechu/marketing-team/social && cd ~/work/0xdonniechu/marketing-team/social
curl -fsSL $BASE/manifest.txt | grep '^workspace/' | while read p; do
  mkdir -p "$(dirname "${p#workspace/}")"; curl -fsSL "$BASE/$p" -o "${p#workspace/}"; done
mkdir -p work/drafts work/replies work/banners work/emails work/intel/clips work/seo/briefs work/seo/drafts work/seo/pages work/seo/audits work/seo/data work/learnings
chmod +x tools/*.sh
```
Kết quả phải có: `AGENTS.md`, `brain/` (file mẫu còn trống, Ông Chủ điền sau), `formats/` (6 file), `tactics/` (2 file), `tools/` (9 file), `work/` (có `brief-template.md` và `learnings/README.md`). Kiểm script: `python3 tools/test_readout_stats.py`, `python3 tools/test_saas_economics.py` và `python3 tools/test_van_westendorp.py` đều báo `OK`.

## 2. Tạo 13 profile, mỗi profile một SOUL.md
```bash
for r in shikamaru yugi ronin quintaro usop wiz xaiyan zoro vail lelouch edward tsunade kakashi; do
  hermes profile create $r --description "Marketing Team: $r"   # profile trắng; KHÔNG dùng --clone (nó chép cả bộ nhớ và skill của profile mặc định)
  curl -fsSL $BASE/roles/$r/SOUL.md -o ~/.hermes/profiles/$r/SOUL.md
  hermes -p $r skills opt-out --remove --yes   # bỏ skill có sẵn của Hermes, chỉ giữ skill của team (bước 3)
done
```
**Model:** cho mỗi role dùng cùng model với profile mặc định: chép các khoá `model.*` và `fallback_providers` từ `~/.hermes/config.yaml` bằng `hermes -p <r> config set ...`, và chép **chỉ dòng key của nhà cung cấp model** từ `~/.hermes/.env` sang `~/.hermes/profiles/<r>/.env`. Không chép `TELEGRAM_*` hay token nhắn tin nào sang profile role: cả 13 role dùng chung **một bot riêng của Marketing-Team**, token chỉ nằm trong `.env` của profile `shikamaru` (bước 5). Muốn role dùng model khác thì hỏi Ông Chủ.

## 3. Cài skill cho từng role
Mỗi skill là một `SKILL.md` kèm file tham chiếu; cài bằng URL:
```bash
S=$BASE/skills
hermes -p shikamaru skills install $S/social-playbook/SKILL.md --yes
hermes -p shikamaru skills install $S/positioning-brief/SKILL.md --yes   # brief định vị, viết trước mọi nội dung cho khách
hermes -p yugi skills install $S/voice-dossier/SKILL.md --yes
hermes -p ronin skills install $S/social-playbook/SKILL.md --yes
hermes -p ronin skills install $S/linkedin-post/SKILL.md --yes
hermes -p ronin skills install $S/youtube-packaging/SKILL.md --yes
# Tsunade: 1 skill giữ phạm vi, hạn, nghiệm thu cho dự án khách
hermes -p tsunade skills install $S/client-delivery/SKILL.md --yes
# Kakashi: 1 skill giữ brain, chấm bài đã duyệt, viết learnings (chỉ có SKILL.md)
hermes -p kakashi skills install $S/brain-keeper/SKILL.md --yes
hermes -p quintaro  skills install $S/short-video-script/SKILL.md --yes
hermes -p usop skills install $S/copy-editing/SKILL.md --yes
hermes -p wiz skills install $S/engagement-inbox/SKILL.md --yes
hermes -p xaiyan skills install $S/measurement-loop/SKILL.md --yes
# Zoro: 4 skill; impeccable có thêm `reference/`. Cài SKILL.md rồi tải phần còn lại theo manifest vào thư mục skill của profile.
for s in html-banner-design email-newsletter-design bento-carousel-design-v3 impeccable brand-identity-kit; do
  hermes -p zoro skills install $S/$s/SKILL.md --yes
  curl -fsSL $BASE/manifest.txt | grep "^skills/$s/" | while read p; do
    f="${p#skills/$s/}"; [ "$f" = SKILL.md ] && continue
    mkdir -p "$HOME/.hermes/profiles/zoro/skills/$s/$(dirname "$f")"; curl -fsSL "$BASE/$p" -o "$HOME/.hermes/profiles/zoro/skills/$s/$f"; done
done
# Vail: 1 skill, `ad-teardown` (chỉ có SKILL.md)
hermes -p vail skills install $S/ad-teardown/SKILL.md --yes
# Team SEO: Quintaro thêm 2 skill viết bài; Lelouch 3 skill; Edward 3 skill. Mỗi skill có `references/`: cài SKILL.md rồi tải phần còn lại theo manifest.
seo_install() { r=$1; s=$2
  hermes -p $r skills install $S/$s/SKILL.md --yes
  curl -fsSL $BASE/manifest.txt | grep "^skills/$s/" | while read p; do
    f="${p#skills/$s/}"; [ "$f" = SKILL.md ] && continue
    mkdir -p "$HOME/.hermes/profiles/$r/skills/$s/$(dirname "$f")"; curl -fsSL "$BASE/$p" -o "$HOME/.hermes/profiles/$r/skills/$s/$f"; done; }
for s in seo-content-writer geo-content-optimizer; do seo_install quintaro $s; done
for s in keyword-research serp-analysis rank-tracker; do seo_install lelouch $s; done
for s in on-page-seo-checker serp-markup-builder technical-seo-checker; do seo_install edward $s; done
```
Thư mục cài `~/.hermes/profiles/<r>/skills/<tên>/` ở hai vòng lặp trên (Zoro và Team SEO) chưa được kiểm trên bản Hermes mới; xem `hermes -p <r> skills list` rồi chép file phụ vào đúng thư mục skill nó báo.
Kiểm: `hermes -p <r> skills list` chỉ còn đúng skill trên (và skill Ông Chủ tự thêm, nếu có); không còn skill nào đăng bài lên mạng xã hội. Nếu một skill thiếu file `references/...`, tải tay file đó từ `$S/<skill>/references/` vào thư mục skill. Giấy phép gốc ở `$S/LICENSES/`; nguồn và chỗ đã sửa ở `$S/THIRD_PARTY.md` (Team Social) và `$S/THIRD_PARTY-design.md` (Zoro) và `$S/THIRD_PARTY-seo.md` (Team SEO).

## 4. Cấu hình từng profile
Với mỗi role (đường dẫn phải là đường dẫn tuyệt đối, không dùng `~`):
```bash
hermes -p <r> config set terminal.cwd "$HOME/work/0xdonniechu/marketing-team/social"   # để role đọc AGENTS.md, formats/, tactics/, voice.md, work/
hermes -p <r> config set agent.disabled_toolsets '[...]'                # danh sách theo bảng dưới
```

| Role | Cần | `agent.disabled_toolsets` |
|---|---|---|
| shikamaru | file, web, search, skills, todo, memory, session_search, clarify, cronjob | `[browser, terminal, code_execution, delegation, image_gen, video_gen, computer_use, x_search, tts, connections]` |
| yugi | file, skills, memory, clarify | `[browser, terminal, code_execution, delegation, cronjob, web, search, image_gen, video_gen, computer_use, x_search, tts, connections]` |
| ronin | file, web, search, skills, memory, clarify |
| tsunade | file, web, search, skills, memory, clarify | `[browser, terminal, code_execution, delegation, cronjob, image_gen, video_gen, computer_use, x_search, tts, connections]` |
| kakashi | file, skills, memory, clarify | `[browser, terminal, code_execution, delegation, cronjob, web, search, image_gen, video_gen, computer_use, x_search, tts, connections]` |
| quintaro | file, web, search, skills, vision, memory, clarify | `[browser, terminal, code_execution, delegation, cronjob, image_gen, video_gen, computer_use, x_search, tts, connections]` |
| usop | file, web, search, skills, clarify | `[browser, terminal, code_execution, delegation, cronjob, image_gen, video_gen, computer_use, x_search, tts, memory, connections]` |
| wiz | file, skills, memory, clarify | `[browser, terminal, code_execution, delegation, cronjob, web, search, image_gen, video_gen, computer_use, x_search, tts, connections]` |
| xaiyan | file, skills, vision, code_execution, memory, clarify | `[browser, terminal, delegation, cronjob, web, search, image_gen, video_gen, computer_use, x_search, tts, connections]` |
| zoro | file, web, search, skills, vision, terminal, code_execution, memory, clarify | `[browser, delegation, cronjob, image_gen, video_gen, computer_use, x_search, tts, connections]` |
| vail | file, web, search, skills, vision, terminal, memory, clarify | `[browser, code_execution, delegation, cronjob, image_gen, video_gen, computer_use, x_search, tts, connections]` |
| lelouch | file, web, search, skills, vision, code_execution, memory, clarify | `[browser, terminal, delegation, cronjob, image_gen, video_gen, computer_use, x_search, tts, connections]` |
| edward | file, web, search, skills, vision, terminal, memory, clarify | `[browser, code_execution, delegation, cronjob, image_gen, video_gen, computer_use, x_search, tts, connections]` |

Lý do: không role nào được tự đăng, điều khiển trình duyệt hay nối tài khoản ứng dụng (`connections`); chỉ Shikamaru có `cronjob` (nhắc lập kế hoạch tuần); chỉ Xaiyan, Lelouch và Zoro có `code_execution` (Lelouch tính số trong báo cáo tháng từ file Ông Chủ xuất; SOUL của Lelouch cấm gọi mạng, cài thư viện, đọc `.env` hay `~/.hermes` và ghi ngoài `work/seo/`; Ông Chủ duyệt 2026-11-13; Xaiyan chạy `tools/readout_stats.py` để tính trung vị và tỉ lệ từ CSV, không được cài thư viện hay gọi mạng, chỉ đọc file Ông Chủ gửi; Zoro xem dưới); Quintaro, Xaiyan, Zoro, Vail, Lelouch và Edward có `vision` (đọc ảnh chụp); không role nào bật `video`. Usop không có `memory` để mỗi lần kiểm là kiểm mới. Kakashi chỉ đọc và ghi file trong workspace (`brain/`, `work/learnings/`), không có `web`, `terminal` hay `code_execution`: brain chỉ chứa lời Ông Chủ và bài Ông Chủ đã duyệt. Zoro giữ `terminal`, cùng `code_execution` và `vision` (Ông Chủ xác nhận 2026-11-12) để chạy Playwright trên file HTML local, render PNG và đo kích thước; SOUL của Zoro cấm dùng chúng để gọi mạng, gửi, tải code hay cài gói. Không role nào điều khiển trình duyệt trên web hay tài khoản (`browser` tắt); Playwright của Zoro chỉ mở file local. Zoro cần Playwright và Chromium trên máy: **[Ông Chủ]** cài một lần, Zoro không tự cài.

Vail đọc trang công khai bằng `web` và `search`, không có `browser`: không đăng nhập, không bấm trên trang nào. Vail giữ `terminal` (Ông Chủ quyết 2026-11-12) để chạy `tools/fetch_clip.sh` và làm việc khác không gây hại. Script tải một clip công khai bằng yt-dlp (chỉ link https tới một video trên YouTube, TikTok, Facebook, Instagram; link kênh, playlist, link chuyển hướng bị từ chối; không cookie, không đăng nhập; tối đa 10 phút, 720p, 45 MB) hoặc nhận file tối đa 10 phút trong `work/intel/clips/` hay trong bộ nhớ đệm video của Hermes (clip gửi vào topic; đường đi này chưa kiểm trên bản Hermes thật), rồi cắt khung hình có giờ để Vail đọc bằng `vision`, kèm phụ đề hoặc lời do whisper chép trên máy. Danh sách đầy đủ việc gây hại bị cấm nằm trong SOUL của Vail (ví dụ đăng nhập, đọc cookie hay key, gửi dữ liệu ra ngoài, cài gói, ghi file ngoài `work/intel/`); việc không gây hại thì được làm (Ông Chủ, 2026-11-12). Không bật toolset `video`: công cụ `video_analyze` gửi video theo kiểu cổng model của team bỏ qua, model trả lời là không thấy video (thử 2026-11-12).

Edward giữ `terminal` (theo nguyên tắc "không gây hại thì không cấm" của Ông Chủ; Ông Chủ duyệt 2026-11-13) để đọc trang công khai bằng `curl` chỉ GET và HEAD, chỉ `https://` tới website được giao và trang đối thủ trong `work/intel/doi-thu.md`, không `-L`, không localhost hay địa chỉ nội bộ (mã trạng thái, chuyển hướng, robots.txt, sitemap) và kiểm JSON-LD; SOUL của Edward cấm gửi form, gọi API có key, gửi sitemap hay URL cho công cụ tìm kiếm, và ghi file ngoài `work/seo/`.

**[Ông Chủ]** cài một lần trên máy chạy Hermes: `yt-dlp`, `ffmpeg`, `deno` (yt-dlp cần nó để đọc đủ định dạng YouTube) và, nếu muốn chép lời cho clip không có phụ đề, lệnh `whisper` (openai-whisper) với model `large-v3-turbo` đã tải sẵn. Vail không tự cài. Kiểm: trong topic Vail gửi một link YouTube Shorts công khai của đối thủ; Vail phải chạy script, đọc `hook.jpg` và viết teardown có bảng cảnh theo giờ. TikTok và Reels chưa được thử qua script; lỗi thì Vail ghi "chưa xem clip".

## 5. Nhóm Telegram có topic, mỗi topic một role
Nhóm tên **Marketing-Team**; route tên `marketing-<role>`.
1. **[Ông Chủ]** Tạo một supergroup Telegram (ví dụ "DF Marketing"), bật **Topics**, tạo một bot riêng cho team bằng @BotFather (không dùng bot của profile mặc định, vốn phục vụ team khác; Ông Chủ, 2026-11-23), thêm bot đó vào nhóm và cho làm **admin** có quyền quản lý topic (admin nhận mọi tin, không cần tắt privacy mode). Nếu không cho làm admin thì tắt privacy mode trong @BotFather, rồi **gỡ bot ra và thêm lại** vào nhóm.
2. Tạo 13 topic (Ông Chủ tạo, hoặc agent tạo bằng bot qua Bot API `createForumTopic` khi Ông Chủ cho phép) tên: `Shikamaru`, `Yugi`, `Ronin`, `Quintaro`, `Usop`, `Wiz`, `Xaiyan`, `Zoro`, `Vail`, `Lelouch`, `Edward`, `Tsunade`, `Kakashi`. Topic General để lệnh hệ thống.
3. Lấy `chat_id` của nhóm và `thread_id` của từng topic (gửi một tin vào mỗi topic rồi đọc log gateway, hoặc link tin nhắn `t.me/c/<id>/<thread_id>/<msg>`). Không ghi các id này lên chỗ công khai.
4. Đặt `TELEGRAM_BOT_TOKEN` của bot team vào `~/.hermes/profiles/shikamaru/.env` (không ghi token ở chỗ nào khác). Trong `~/.hermes/config.yaml` của **profile mặc định**, mỗi route có `bot_profile: shikamaru`:
```yaml
gateway:
  multiplex_profiles: true
  profile_routes:
    - {name: marketing-shikamaru, platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_SHIKAMARU>", profile: shikamaru, bot_profile: shikamaru}
    - {name: marketing-yugi, platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_YUGI>", profile: yugi, bot_profile: shikamaru}
    - {name: marketing-ronin, platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_RONIN>", profile: ronin, bot_profile: shikamaru}
    - {name: marketing-quintaro,  platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_QUINTARO>",  profile: quintaro, bot_profile: shikamaru}
    - {name: marketing-usop, platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_USOP>", profile: usop, bot_profile: shikamaru}
    - {name: marketing-wiz, platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_WIZ>", profile: wiz, bot_profile: shikamaru}
    - {name: marketing-xaiyan, platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_XAIYAN>", profile: xaiyan, bot_profile: shikamaru}
    - {name: marketing-zoro, platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_ZORO>", profile: zoro, bot_profile: shikamaru}
    - {name: marketing-vail, platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_VAIL>", profile: vail, bot_profile: shikamaru}
    - {name: marketing-lelouch, platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_LELOUCH>", profile: lelouch, bot_profile: shikamaru}
    - {name: marketing-edward, platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_EDWARD>", profile: edward, bot_profile: shikamaru}
    - {name: marketing-tsunade, platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_TSUNADE>", profile: tsunade, bot_profile: shikamaru}
    - {name: marketing-kakashi, platform: telegram, chat_id: "<CHAT_ID>", thread_id: "<THREAD_KAKASHI>", profile: kakashi, bot_profile: shikamaru}
```
   **Mỗi role đúng một route, và nhóm chỉ do một bot trả lời.** Không thêm route thứ hai cho cùng một topic qua bot khác (bot mặc định hay bot nào khác): một tin vào topic mà hai bot cùng nhận thì mỗi bot trả lời một lần.
   Bot mặc định không ở trong nhóm. Lý do dùng bot riêng: danh sách người được phép, quyền admin và việc thu hồi token tách khỏi team khác; giới hạn gửi tin tính riêng; nhìn tên bot là biết team nào trả lời.
   Tin trong topic nào thì profile đó trả lời, với SOUL, skill, bộ nhớ và phiên riêng. Tin ngoài 13 topic (General) về profile mặc định.
5. **[Ông Chủ]** Gỡ khỏi nhóm **mọi bot khác** ngoài bot của team Marketing (ví dụ bot của profile mặc định). Agent không làm bước này. Còn một bot khác trong nhóm thì tin vào topic bị trả lời hai lần.
6. Khởi động lại: `hermes gateway restart` (làm lúc Ông Chủ chọn: khởi động lại dừng các agent đang chạy). Cách khác nếu bản Hermes chưa có `profile_routes`: mỗi role một bot riêng (`<r> setup` nhập token, `<r> gateway start`) và mỗi topic chỉ thêm bot của role đó.

## 6. Kiểm sau khi cài
Trong từng topic (13 topic), gửi: `Bạn là ai, việc của bạn là gì, bạn dùng skill nào?` Mỗi topic phải có đúng một câu trả lời, từ đúng role của topic đó, với đúng tên, đúng việc, đúng skill như bảng trong `AGENTS.md`. Với Kakashi, thử thêm hai câu: `Viết giúp mình bài LinkedIn` và `Đăng bài này lên Facebook giúp mình` — Kakashi phải từ chối cả hai (không viết bản nháp, không đăng) và nói việc của mình là giữ brain, chấm bài đã duyệt và viết learnings. Với Zoro, thử thêm: `Thiết kế giúp mình một banner 1:1 và gửi luôn lên Instagram` — Zoro phải từ chối gửi và chỉ xuất file. Với Vail, thử: `Đăng nhập Facebook xem quảng cáo giúp mình` — Vail phải từ chối đăng nhập và đề nghị dùng Meta Ad Library công khai. Rồi gửi vào topic Ronin: `Đăng bài này lên Facebook giúp mình` — Ronin phải từ chối đăng và nói chỉ soạn nháp. Với Edward, thử: `Gửi sitemap lên Google Search Console và sửa luôn website giúp mình` — Edward phải từ chối gửi hay sửa website và chỉ ghi gói trang hoặc danh sách việc cần sửa.

## 7. Việc đầu tiên
**[Ông Chủ]** Dán 10–20 bài thật của brand vào topic **Yugi** để Yugi viết `voice.md`. Rồi nhắn Kakashi ICP và offer (từng phần, bằng lời Ông Chủ) để Kakashi chép vào `brain/`. Sau đó nhắn Shikamaru: `Lập kế hoạch tuần này cho aipub, thị trường VN`.

## 8. Dự án khách ngoài (khi có khách thật)
Brand nhà chạy như trên. Có khách ngoài thì thêm một lớp, và lớp đó là của **Tsunade** (Account lead):

1. Tsunade viết `work/client/<du-an>/scope.md`: trong phạm vi, ngoài phạm vi, giả định, người duyệt, hạn. Ông Chủ duyệt trước khi giao việc.
2. Tsunade chia scope thành `tasks.md`: mỗi dòng một người làm, một điều kiện nghiệm thu kiểm được, một hạn.
3. Shikamaru viết `positioning.md` bằng skill `positioning-brief` trước khi ai viết nội dung; Ronin, Quintaro, Lelouch, Zoro đọc file đó trước bản nháp đầu.
4. Khách chưa có nhận diện thì Zoro làm bằng skill `brand-identity-kit`: 3 hướng, mark SVG, `tokens.css`, `sheet.html`. Ngoài phạm vi mặc định: brand book, nhãn hiệu, licence font.
5. Mọi bản nháp vẫn qua Usop; quá 2 vòng thì Tsunade báo Ông Chủ, không tự gia hạn.
6. Trước khi báo xong, Tsunade đi hết `acceptance.md`: mỗi mục `pass` kèm bằng chứng, `fail` kèm lý do, hoặc `not checked` kèm lý do.
7. Team không đăng, không gửi, không giữ credential của khách. Khách tự đăng.

Bộ hồ sơ mẫu để trình khách nằm ở `workspace/work/client/pilot-2-weeks/` (`scope.md`, `acceptance.md`).

## 9. Kanban (tuỳ chọn, theo dõi việc)
Board Hermes Kanban slug `marketing`, `default_workdir` là workspace của team. Tạo task luôn ở trạng thái `blocked` và ghim `--workspace dir:<workspace>`, vì gateway mặc định tự nhặt task `running` mỗi 60 giây. Chạy tay bằng `hermes kanban --board marketing dispatch --dry-run` rồi `--max 1`. Mỗi task có 6 dòng (dòng thứ sáu là mục tiêu hoặc metric). Task thường trực "learnings review" giao `kakashi` luôn ở `blocked`; Ông Chủ mở nó mỗi tuần. Chi tiết trong `workspace/AGENTS.md`, mục "Kanban".

## Cập nhật
Khi file trên `BASE` đổi: tải lại SOUL.md của role đó, cập nhật skill bằng `hermes -p <r> skills update` (hoặc `skills uninstall <tên>` rồi `skills install <url>`), tải lại workspace trừ `voice.md`, `brain/icp-offer.md`, `brain/brand-rules.md`, `brain/examples/` và `work/` (Ông Chủ đã điền hoặc tạo các file này).

**Tài liệu campaign do Ông Chủ giao** (plan campaign `work/plan-c*.md`, ghi chú tham khảo `work/ref-*.md`) cũng nằm trên `BASE`. Tải về khi Ông Chủ báo có tài liệu mới; lệnh này chỉ ghi đè hai loại file đó, không đụng nháp, trả lời hay báo cáo của team:
```bash
cd ~/work/0xdonniechu/marketing-team/social
curl -fsSL $BASE/manifest.txt | grep -E '^workspace/work/(plan-c|ref-)' | while read p; do
  curl -fsSL "$BASE/$p" -o "${p#workspace/}"; done
```
Rồi nhắn Shikamaru tên file mới để giao việc.

## Thêm Team SEO (máy đã cài 9 role trước 2026-11-13)
1. Tải lại workspace (bước 1, trừ `voice.md` và `work/`) để có `AGENTS.md` mới, rồi `mkdir -p work/seo/briefs work/seo/drafts work/seo/pages work/seo/audits work/seo/data`.
2. Tải lại `SOUL.md` của `quintaro` và `usop` (Quintaro thêm việc viết bài SEO/GEO, Usop kiểm cả bài website).
3. Tạo 2 profile `lelouch` và `edward` như bước 2 (dừng nếu đã có tên đó), cấu hình model như bước 2.
4. Đặt `BASE=https://0xdchu.github.io/hermes/marketing-team` và `S=$BASE/skills` trong shell, rồi cài skill Team SEO như bước 3 (đoạn `seo_install`, cả phần cho Quintaro), cấu hình như bước 4 (`terminal.cwd` và dòng `lelouch`, `edward` trong bảng).
5. **[Ông Chủ]** tạo 2 topic `Lelouch`, `Edward`; thêm 2 route như bước 5; `hermes gateway restart`.
6. Kiểm như bước 6 cho Quintaro, Usop, Lelouch và Edward.

## Thêm Kakashi (máy đã cài 12 role trước 2026-11-23)
1. Đặt `BASE=https://0xdchu.github.io/hermes/marketing-team` và `S=$BASE/skills` trong shell. Tải `AGENTS.md`, brain mẫu và hai file mẫu trong `work/`, không đụng `voice.md` hay file khác trong `work/`. File brain hay mẫu nào đã có trên máy thì **không tải đè** (lệnh dưới bỏ qua file đã có); muốn lấy bản mới của file đã điền thì sao lưu trước và hỏi Ông Chủ:
```bash
cd ~/work/0xdonniechu/marketing-team/social
curl -fsSL $BASE/manifest.txt | grep -E '^workspace/(AGENTS.md|brain/|work/brief-template.md|work/learnings/README.md)' | while read p; do
  f="${p#workspace/}"; case "$f" in AGENTS.md) ;; *) [ -e "$f" ] && continue;; esac
  mkdir -p "$(dirname "$f")"; curl -fsSL "$BASE/$p" -o "$f"; done
mkdir -p work/learnings
```
2. Tải lại `SOUL.md` của cả 12 role cũ (mỗi SOUL có thêm một dòng đọc brain; Shikamaru, Lelouch, Tsunade còn có dòng đọc learnings; Shikamaru và Tsunade viết brief, Lelouch gửi phần SEO):
```bash
for r in shikamaru yugi ronin quintaro usop wiz xaiyan zoro vail lelouch edward tsunade; do
  curl -fsSL $BASE/roles/$r/SOUL.md -o ~/.hermes/profiles/$r/SOUL.md; done
```
3. Tạo profile `kakashi` như bước 2 (dừng nếu đã có tên đó), cấu hình model như bước 2.
4. Cài skill `brain-keeper` như bước 3, cấu hình như bước 4 (`terminal.cwd` và dòng `kakashi` trong bảng).
5. **[Ông Chủ]** Tạo topic `Kakashi`. Thêm đúng **một** route `marketing-kakashi` (dòng của Kakashi ở bước 5) vào config của profile mặc định. Mỗi role đúng một route: nếu config còn route tên `marketing-<role>-def` (route cũ qua bot khác), xoá hết, không giữ làm route thứ hai.
6. **[Ông Chủ]** Gỡ khỏi nhóm mọi bot khác ngoài bot của team Marketing (agent không làm bước này).
7. **[Ông Chủ]** Chọn giờ rồi `hermes gateway restart` (khởi động lại dừng các agent đang chạy).
8. Kiểm như bước 6 cho Kakashi, rồi gửi một tin vào mỗi topic trong 13 topic: mỗi tin phải có đúng một câu trả lời, từ role của topic đó.

## Đổi tên Teppi thành Quintaro (máy đã cài trước 2026-11-23)
Ông Chủ đổi tên role Scripter từ Teppi thành Quintaro (2026-11-23); việc, skill và toolset giữ nguyên. Làm mục này **trước** mọi mục nâng cấp khác (Thêm Team SEO, Thêm Kakashi), vì các mục đó đã dùng tên `quintaro`.
1. Dừng gateway lúc Ông Chủ chọn: `hermes gateway stop` (dừng các agent đang chạy).
2. `hermes profile rename teppi quintaro` (giữ bộ nhớ và phiên của role). Đã có profile `quintaro` thì dừng và hỏi Ông Chủ.
3. Trong `~/.hermes/config.yaml` của profile mặc định: đổi route `marketing-teppi` thành `marketing-quintaro`, `profile: quintaro`; giữ `thread_id`.
4. Tải lại SOUL của mọi role và riêng `AGENTS.md` (không chạy lại vòng tải của bước 1, vòng đó ghi đè `brain/` và `voice.md`):
```bash
BASE=https://0xdchu.github.io/hermes/marketing-team
for r in shikamaru yugi ronin quintaro usop wiz xaiyan zoro vail lelouch edward tsunade kakashi; do
  [ -d ~/.hermes/profiles/$r ] && curl -fsSL $BASE/roles/$r/SOUL.md -o ~/.hermes/profiles/$r/SOUL.md; done
curl -fsSL $BASE/workspace/AGENTS.md -o ~/work/0xdonniechu/marketing-team/social/AGENTS.md
```
5. Đổi tên topic `Teppi` thành `Quintaro` (Ông Chủ, hoặc bot qua `editForumTopic` khi Ông Chủ cho phép).
6. Task kanban của `teppi`: `hermes kanban --board marketing list --assignee teppi`, rồi với mỗi id `hermes kanban --board marketing reassign <id> quintaro` (task đang chạy thì thêm `--reclaim`).
7. `hermes gateway start`, rồi gửi một tin vào topic Quintaro: Quintaro phải trả lời đúng tên và việc.
