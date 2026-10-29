# Red Hedge — audit tích hợp (bản COPY, không phải bản gốc)

- Nguồn gốc: `~/workspace/games/meowburger-source` (Meow Burger v1.6.4) — **không bị sửa**.
- Bản làm việc: `~/workspace/games/red hedge` — bản HTML cuối: `dist/index.html` (8,8 MB),
  `dist/artifact.html`, `dist/minis/index.html` (9,3 MB thư mục minis).
- Nguyên tắc: chỉ đổi text người chơi nhìn thấy và art thay được đúng format.
  Key nội bộ, save key, SKU, placement key giữ nguyên (xem mục "Cố tình giữ").
- Bản HTML này **chưa được mở kiểm tra trong trình duyệt thật** (subagent không có
  trình duyệt live); cần parent kiểm tra trực tiếp trước khi giao cho Andy.

## 1. File đã thay (tổng kết)

| Nhóm | Số lượng | Chi tiết |
|---|---|---|
| Nhân vật (sprite sheet) | 224 pose | `src/16_more_sprites.js` viết lại từ bộ nhím `~/workspace/work/nhim-characters-full/`, giữ nguyên key/pose của game. Xem bảng ánh xạ mục 2. |
| Minis UI | 12 file | `assets/minis/`: loading_bg, icon_192, icon_512, btn_home, btn_revisit, btn_share, paw_thumb (đã là mặt nhím, giữ tên file vì build.py/template tham chiếu), pop_home_art, pop_home_steps, pop_revisit_profile, pop_revisit_coins, pop_share_art. Tất cả lấy từ Red Hedge v2, đã thu nhỏ (tổng 2,7 MB so với 1,1 MB gốc). |
| ENV sprite (code art) | 11 key | `src/18_env_art.js`: bin←trash_bin_red hedge, plant←plant_jp, planter←planter_jp, signstand←sign_stand_jp, boxes←boxes_jp, register←cash_register_jp, carrytray←carry_tray_jp, menuC←menu_board_chicken_jp, menuB←menu_board_burger_jp, menuS←menu_board_salad_jp, fridge←fridge_red hedge. Giữ nguyên w/h/tip gốc, ảnh fit-trong-khung + đệm trong suốt, WebP q82. |
| Text hiển thị | 7 file | `src/00_config.js`, `src/05_platform.js`, `src/60_ui.js`, `src/65_minis_ui.js`, `src/70_main.js`, `src/index.template.html`, `README.md` (bản copy). |
| Art vẽ bằng code | 6 file | `src/10_draw.js` (pawPrint→mặt nhím), `src/50_render.js` (biển PICK UP: mặt mèo→mặt nhím), `src/30_world.js` (cửa mèo trên tường→cửa nhím; thêm rèm noren sakura + đèn lồng giấy, chỉ trang trí), `src/65_minis_ui.js` (MX_ICON.paw SVG→SVG mặt nhím), `src/21_items.js` (hộp takeaway 'MEOW'→'HEDGE'; ly nước: tai mèo trắng→gai nhím nâu, giữ mắt), `src/26_stations_code.js` (dải mái hiên thứ hai đổi sang hồng sakura #ffe9ee). |
| Comment/header | 8 file | `src/22_objects.js`, `23_assets.js`, `24_env.js`, `26_stations_code.js`, `30_world.js`, `50_render.js`, `17_pizza_art.js`, `18_env_art.js` — đổi "Meow Burger" thành Red Hedge trong comment mô tả game (không phải text hiển thị). |

## 2. Ánh xạ nhân vật 224/224 (đã kiểm tra bằng node parse)

| Key game | Nguồn nhím | Pose |
|---|---|---|
| chef (72) | 01_player_chef | idle, walk1–4, cwalk1–4, đủ 8 hướng |
| cashier (12) | 02_cashier | idle 8 hướng + walk1–4_270 |
| waiter (12) | 03_runner_food_and_kitchen | idle 8 hướng + walk1–4_270 (cả vai trò food-runner và kitchen-runner đều dùng skin 'waiter') |
| cleaner (12) | 04_cleaner | idle 8 hướng + walk1–4_270 |
| manager (16) | 05_manager | idle 8 hướng + tablet1–4 + walk1–4_270 |
| cat01 (20) | 06_customer_1 | idle 8 + walk1–4_270 + sit1–4/eat1–4 |
| cat02 (16) | 07_customer_2 | idle 8 + walk1–4_270 + sit_eat_seat1–4 |
| cat03 (16) | 08_customer_3 | như cat02 |
| cat04 (16) | 09_customer_4_purple_dress | như cat02 |
| vip01 (16) | 10_vip_1 | như cat02 |
| vip02 (16) | 11_vip_2 | như cat02 |

- Cách chuyển: lấy alpha bounding box của PNG nhím, resize sao cho chiều cao
  bounding box = 230 px, WebP q82; tính lại w/h/ax/ay/legY/s từ bounding box
  (ax = tâm ngang bounding box, ay = đáy bounding box, legY ≈ 84% chiều cao
  bounding box — **là ước lượng**, xem giới hạn). Trường `tray` giữ nguyên
  (tọa độ tương đối). Tổng file sprite mới 4,8 MB (gốc 4,1 MB).
- `src/15_staff_sprites.js` vẫn rỗng (`STAFF_SPRITES={}`) — không có gì để thay;
  nhánh `drawStaff()` trong `src/50_render.js` không bao giờ chạy trong build này.

## 3. Bảng rà soát đầy đủ (file + dòng/vị trí + kết quả + lý do)

### 3a. Text hiển thị — ĐÃ ĐỔI
| Vị trí | Cũ | Mới |
|---|---|---|
| template dòng 1, 158, 182 | title "Meow Burger", aria-label, alt | "Red Hedge" |
| 00_config: stageNames, SPOTS renovate, TASKS | Meow Burger Diner / Cashier cat / Cleaner cat / Cat Cola / Hire a cashier cat / Hire a cleaner cat / Renovate into Meow Burger Diner / New drink: Cat Cola | Red Hedge Diner / Cashier hedgehog / Cleaner hedgehog / Hedge Cola / …hedgehog / Renovate into Red Hedge Diner / New drink: Hedge Cola |
| 05_platform: mock add-to-home, mock share, ad label | Meow Burger / Meow Rush | Red Hedge / Hedge Rush |
| 60_ui: shop row, toast, milestone | Meow Rush / Meow Burger is now a Burger Palace! | Hedge Rush / Red Hedge is now a Burger Palace! |
| 65_minis_ui: 5 popup text | Meow Burger … | Red Hedge … |
| 70_main: loading | Calling the cats… / Ready! Meow! | Calling the hedgehogs… / Ready! Red Hedge! |
| README (bản copy) | # Meow Burger, mô tả Meow Burger | # Red Hedge, mô tả Red Hedge (giữ ghi chú lịch sử engine Meow Pizza) |

Kiểm chứng trên `dist/index.html`: "Meow Rush"=0, "Cat Cola"=0, "cashier cat"=0,
"cleaner cat"=0, "Calling the cats"=0, "Ready! Meow"=0, "Meow Burger Diner"=0,
"Red Hedge"=28, "Hedge Cola"=2, "Hedge Rush"=5. Lần "Meow Burger" còn lại duy
nhất là comment `Map zones (Red Hedge v1.2, ex-Meow Burger)` — ghi chú lịch sử,
không hiển thị cho người chơi.

### 3b. Art vẽ bằng code — ĐÃ ĐỔI
| Vị trí | Cũ | Mới |
|---|---|---|
| 10_draw.js:45 `pawPrint()` | dấu chân (đệm lớn + 4 ngón) | mặt nhím: đĩa gai nâu #8a5a2c (10 gai) + mặt kem #fff4e3 + mắt + má hồng. Tên hàm giữ nguyên để các call site không vỡ. Màu tham số `c` không còn dùng cho mặt nhím (ghi trong comment). |
| 22_objects.js:18 `counterBase()` | gọi pawPrint trên mặt quầy | tự động thành mặt nhím nhờ pawPrint mới |
| 22_objects.js:155 bin, :157 office | gọi pawPrint | tự động thành mặt nhím |
| 50_render.js:154 queue slots | `pawPrint(..., '#fff')` cho mỗi slot chờ | tự động thành mặt nhím |
| 20_rig.js:110,178 apron logo | gọi pawPrint trên tạp dề rig | tự động thành mặt nhím (rig fallback, xem 3d) |
| 50_render.js `drawCounterSeg()` biển PICK UP | vòng tròn trắng + 2 tai tam giác (mặt mèo) | đĩa gai nâu + mặt kem + mắt, giữ chữ 'PICK UP' và biển đỏ |
| 30_world.js `drawWalls()` cửa tường phải | khung cửa + mặt kem ellipse + 2 tai tam giác + 2 mắt | khung cửa giữ nguyên, mặt kem + đĩa gai nâu + mắt (cửa nhím) |
| 65_minis_ui.js:9 `MX_ICON.paw` | SVG dấu chân (ellipse + 4 tròn) | SVG mặt nhím (đĩa gai + mặt kem + mắt). Lưu ý: icon này hiện không được dùng trong file (chỉ share và x được dùng) — đổi để không còn SVG dấu chân nào trong code. |
| 21_items.js `pizzaBox()` | chữ 'MEOW' trên hộp takeaway | 'HEDGE' — chữ "Red Hedge" không vừa vòng tròn bán kính T*.09 ở cỡ chữ T*.07 nên dùng dạng ngắn, nói thẳng. |
| 21_items.js `drink` (ly Cat Cola) | ly trắng + 2 tai tam giác trắng + mắt | tai đổi thành 3 gai nâu #8a5a2c trên miệng ly, giữ mắt — đọc thành ly Hedge Cola có gai nhím |
| 26_stations_code.js `ST_THEME.aw` | dải mái hiên thứ hai '#fff6ea' (kem) | '#ffe9ee' (kem hồng sakura). Màu biển hiệu, icon món, chữ (CHICKEN/BURGER/DELUXE/CK BURGER/SALAD/FRIES/SODA), vị trí, chức năng giữ nguyên. |
| 30_world.js `drawWalls()` trang trí Nhật | (không có) | 3 rèm noren hồng #f9c5d1 có hoa sakura + 3 đèn lồng giấy trên tường sau. Chỉ trang trí, không đổi hình học sàn/tường, không đổi collision. |

### 3c. Art vẽ bằng code — ĐÃ KIỂM TRA, KHÔNG CÓ MÈO (không đổi)
- 21_items.js: các painter pizza/pepperoni/veggie/seafood/special, topping, coin
  (coin vàng trơn, không đầu mèo), emotes (heart/anger/sweat/zzz/sparkle/bang/check)
  — không mèo.
- 22_objects.js: các painter ctr_tray/ctr_shelf/ctr_plant/ctr_register/ctr_boxes,
  stanchion, table (bàn vuông kem/chân đen), chair, vip, dc_planter, dc_board
  (bảng "MENU" — đã kiểm tra trực tiếp dòng 169–172: dưới chữ MENU chỉ có 1
  tam giác viền vàng + 2 chấm đỏ nhỏ, không có vòng mặt/tai/râu nên không đọc
  thành mặt mèo; giữ nguyên, ghi nhận là chi tiết mơ hồ chứ không khẳng định
  sạch tuyệt đối),
  dc_plant, grand ("GRAND OPENING"), ribbon, rail, lock_sign, gd_tree/gd_bush/
  gd_lamp/gd_bench/gd_flowers/gd_pot — không mèo, không dấu chân (trừ 3 chỗ
  pawPrint đã đổi ở 3b).
- 26_stations_code.js: icon biển hiệu là icon món (đùi gà, burger, salad, khoai,
  ly) — không mèo, không tai/râu.
- 30_world.js: sàn gạch kem, sàn gỗ, tường gạch be, cây code `drawTreeP1()` — không mèo.
- 50_render.js: các phần còn lại (bàn, quầy, kệ, nhân viên cầm khay) — không mèo
  ngoài biển PICK UP và queue slot đã đổi.

### 3d. Sprite rig vẽ bằng code — CHƯA THAY ĐƯỢC 1:1 (nói thẳng)
- `src/20_rig.js` (185 dòng): rig "CHUBBY cat rig" vẽ từng bộ phận bằng code:
  đầu có tai (`drawEar`, dòng 127/131/137), đuôi (dòng 43–44, 46, 60, 86–87),
  râu/mặt, mũ, kính, ba lô, tạp dề. `CAT_SKINS` gồm chef/waiter/cashier/helper/
  manager/c_grey/c_ginger/c_tux/c_calico/c_cream/c_elder/kid/tourist/vip với
  trường `ear`/`inner` và tên "Chef Cat (player)", "Waiter Cat", …
- Lý do không thay 1:1: đây không phải file ảnh mà là code vẽ đầu/tai/đuôi/mặt;
  muốn thành nhím phải viết lại `drawRigCat()` (đầu gai, bỏ tai/đuôi/râu) — ngoài
  phạm vi "thay ảnh đúng format" và rủi ro vỡ animation walk/cwalk/serve/cook/…
- Giảm nhẹ thực tế: mọi skin dùng trong gameplay (chef, cashier, waiter, cleaner,
  manager, cat01–cat04, vip01–vip02) đều có sheet trong `MORE_SPRITES` nên
  `drawChar()` (50_render.js:118–123) không bao giờ rơi xuống rig. Rig chỉ là
  fallback + dùng để bake preview strip trong `src/23_assets.js` (`bakeRigPreview`).
  Logo tạp dề của rig đã thành mặt nhím nhờ pawPrint mới. Tên `CAT_SKINS`,
  hàm `drawRigCat`, tên skin "… Cat" vẫn còn trong code (nội bộ, không hiển thị).

### 3e. Assets PNG tham chiếu — CHƯA THAY (không load trong build web)
Các file này chỉ dùng khi bật `window.CAT_PIZZA_USE_FILES` (file mode) hoặc làm
preview/QA; build web bình thường dùng sprite data/code painter, nên thay chúng
không đổi hình ảnh trong HTML. Vì vậy không ghi đè chúng bằng ảnh minh họa v2
(khác kích thước/anchor/chức năng — ghi đè sẽ làm file mode vẽ sai hơn).
- `assets/characters/` 59 file: 27 file `cat_*.png` (8dir preview 1024×186 và
  strip 3172–3294×169: cat_chef/cashier/waiter/waitress/manager/vip/tourist/
  c_grey/c_ginger/c_calico/c_cream/c_dark/c_white/c_elder/c_tux/helper/kid)
  — vẫn là mèo, chỉ là preview, không load. 32 file staff preview
  (chef/cashier/waiter/waitress idle/walk) cũng là preview tham chiếu, không load
  (STAFF_SPRITES rỗng).
- `assets/restaurant/` 26 file, `assets/stations/` 8 file, `assets/outdoor/` 6 file,
  `assets/items/` 21 file, `assets/ui/` 7 file emote — preview tham chiếu cho
  painter code tương ứng. Trong đó `assets/items/it_drink.png` (ly 72×72) vẫn
  mang khuôn mặt ly kiểu cũ và `it_box.png` là hộp pizza — nhưng runtime dùng
  painter code (đã sửa ở 3b), không dùng 2 file này. V2 không có file footprint
  tương đương 1:1 cho các sprite này (ảnh v2 là minh họa toàn cảnh, không phải
  sprite chân đế), nên không thay.
- `assets/manifest.json`: vẫn mô tả game là "Cat Pizza Tycoon" v3.0.0 với các
  entry `cat_*` rig — file tham chiếu, không load trong build web; không sửa
  để khỏi phá quy tắc "cùng kích thước/anchor" mà manifest này ghi.

### 3f. ENV sprite — ĐÃ THAY 11 key, CÒN LẠI LÀ CODE CHẾT (không thay)
Đã thay (mục 1): bin, plant, planter, signstand, boxes, register, carrytray,
menuC, menuB, menuS, fridge. Lưu ý: `planter` và `fridge` trong ENV không có
call render trong luồng chính (decor planter dùng `drawPlanter()` code, fridge
không có call) — vẫn thay để file data đồng bộ, nhưng hình trong game ở 2 chỗ
này vẫn là code.
Không thay, vì là code chết trong render chính (đã grep `drawEnv`/`drawEnvAt`
trong 30_world.js/50_render.js: không có call): chicken, burger, burger2, salad,
drink, fryer (station ENV, `stationArt()` không được `drawStation()` gọi —
station thật vẽ bằng `drawCodeStation()`), ctrL/ctrC/ctrR, table, table_front,
trayR, trayG, qpost, ckbox, shelf0–shelf3 (icon mức tồn kho của pantry, không
phải đồ nội thất — ảnh kệ v2 là minh họa kệ cao, khác chức năng/kích thước).
Quầy/bàn/kệ thật trong game vẫn là code painter (đã restyle Nhật ở 3b), không
phải ảnh v2.

### 3g. Cố tình giữ nguyên (nội bộ, không hiển thị cho người chơi)
- `SAVE_KEY='meowburger.v3'`, `LEGACY_SAVE_KEY='meowburger.v0'` (00_config:18)
  — đổi sẽ mất save cũ.
- Placement key `'meow_rush'`, global `window.MeowBurgerAds`, `window.CAT_PIZZA_USE_FILES`,
  biến/hàm `CAT_SKINS`, `drawRigCat`, `catOf()`, trường `ITEMS[].cat` ('food'/'ing'/'trash' —
  đây là category, không phải mèo), key sprite `cat01`–`cat04` trong MORE_SPRITES/SHEET_H/
  SHEET_REMAP — đổi tên đồng loạt rủi ro vỡ spawn/anim; key không hiển thị.
- Tham chiếu lịch sử engine: "Meow Pizza v4.8 engine", "Meow Sushi ads config"
  (comment trong 00_config/05_platform) và comment "ex-Meow Burger" — lịch sử,
  không phải thương hiệu game hiện tại.
- `VERSION='1.6.4'` giữ nguyên (phiên bản, không phải tên).
- TikTok App ID / HS Game ID `7699569778830231573` giữ nguyên (config SDK).
- `assets/manifest.json` giữ nguyên (xem 3e).

## 4. Build & test
- `python3 src/build.py` lần đầu: FAIL ở bước esbuild vì binary mặc định
  `/opt/npm-tools/node_modules/esbuild/bin/esbuild` không tồn tại trên VM này
  (lỗi môi trường sẵn có, không phải do sửa đổi). Chạy lại với esbuild cài ở
  `/tmp/node_modules/.bin/esbuild` (v0.28.2): **EXIT 0**.
- `node --check dist/game.js`: **OK**. `node --check` mọi file `src/*.js`: **OK**.
- `node tests/qa_native.js`: **không chạy được** — cần module playwright ở
  `/opt/npm-tools/node_modules/playwright`, thư mục này không tồn tại trên VM.
  Các test khác (qa_minis, saveload, near, orient, econtable) cũng cần playwright
  hoặc file botlog cũ nên không chạy được — đây là giới hạn môi trường, không
  phải lỗi build. **Không chạy test playthrough 40 phút** theo chỉ thị.
- Kết quả dist: `dist/index.html` 8,8 MB, `dist/artifact.html` 8,8 MB,
  `dist/game.js` 7,4 MB, `dist/minis/index.html` 8,8 MB, icon_192 80 KB,
  icon_512 446 KB.
- Nguồn gốc `~/workspace/games/meowburger-source` không thay đổi (kiểm tra:
  vẫn còn "Cat Cola", không có "Red Hedge", timestamp gốc 2026-10-24).
- Không push GitHub, không tạo cron, không nhắn cho người dùng.

## 5. Giới hạn phải nói thẳng với Andy
1. Nhân vật rig code (`src/20_rig.js`) vẫn vẽ mèo bằng code — chỉ là fallback,
   không xuất hiện trong gameplay hiện tại vì mọi skin đều có sheet nhím, nhưng
   preview rig và tên skin nội bộ vẫn là mèo.
2. Ảnh nhím chỉ thực sự thay ở: nhân vật trong game (224 pose), minis UI (12 file),
   ENV prop (11 key), và các chi tiết code đã liệt kê ở 3b. Ảnh v2 toàn cảnh
   (overview/P1 scene/station illustration) không thể nhét 1:1 vào game vì game
   vẽ quầy/bàn/trạm bằng code — nhà hàng Nhật trong HTML là restyle màu/decor
   (noren, đèn lồng, mái hiên sakura, prop Nhật) chứ không phải ảnh v2 nguyên tấm.
3. `legY` của sprite nhím là ước lượng (84% chiều cao bounding box) — dùng cho
   hiệu ứng bước chân ở hướng không có frame walk vẽ sẵn; có thể cần chỉnh tay
   nếu thấy chân trượt.
4. File PNG tham chiếu trong `assets/` (cat_8dir, restaurant, stations, items…)
   vẫn còn mèo — chúng không load trong build web, nhưng nếu ai bật file mode
   thì sẽ thấy mèo cũ.
5. Bản HTML chưa được mở kiểm tra trong trình duyệt thật — cần kiểm tra trực
   tiếp (framing nhân vật, biển PICK UP, cửa nhím, noren/đèn lồng, popup minis)
   trước khi giao.
6. Không có thay đổi nào được push lên GitHub; repo `ybdstudio/meow2aistudio`
   và `ybdstudio/meowburger` không bị động tới.

## 6. Round 2 — giải quyết 5 giới hạn (2026-10-25, bản HTML mới 11 MB)

### 6a. Rig mèo -> rig nhím (src/20_rig.js)
- HEDGE_SKINS thay CAT_SKINS; drawRigHedgehog thay drawRigCat (call site ở 23_assets.js, 50_render.js đã đổi theo); tên skin: Chef/Cashier/Waiter/Manager Hedgehog.
- Bỏ drawEar, bỏ tai/râu mèo, bỏ đuôi mèo cong; thay bằng đĩa gai 14 gai nâu #8a5a2c quanh đầu (cả mặt sau vẫn đọc là nhím) + mặt kem C.white, đuôi chỉ còn nub nhỏ #8a5a2c không vẫy.
- Bỏ patch calico/tabby trên đầu. Animation rigPose (walk/cwalk/carry/serve/cook/reach/register/happy/wait/tired/mad/sit/eat/sleep) giữ nguyên, chỉ đổi phần vẽ đầu/đuôi.
- itemKindOf thay catOf ở 40_game.js, 45_autoplay.js, 50_render.js, 70_main.js; window.RED_HEDGE_USE_FILES thay CAT_PIZZA_USE_FILES ở 23_assets.js.
- Cố tình giữ: key sprite MORE_SPRITES cat01-cat04 (key spawn/anim, không hiển thị), trường ITEMS[].cat (category food/ing/trash), SAVE_KEY meowburger.v3 (đổi sẽ mất save), window.MeowBurgerAds (tên global SDK), comment lịch sử Meow Pizza/Meow Sushi/ex-Meow Burger, và tên file manifest cat_*_8dir.png (bakeRigPreview cắt 4 ký tự đầu key; ảnh trong file giờ đã là nhím, xem 6d).

### 6b. Ảnh v2 vào game thật (src/19_v2_art.js mới, 1,38 MB WebP q82)
- Trạm: drawV2Station() trong 50_render.js vẽ ảnh v2 đứng trên footprint (rộng = (w+d)*TW/2, đáy ở góc gần footprint) thay cho drawCodeStation(); nếu ảnh chưa load thì fallback về code painter. Điểm nhận đồ, vòng tiến độ, số lượng, chấm nguyên liệu vẫn vẽ sau ảnh nên chức năng không đổi.
  - oven -> station_chicken_jp, prep -> station_burger_jp, special -> station_burger_small_jp, seafood -> station_burger_jp (dùng lại, ghi rõ), veggie -> station_salad_jp, dessert -> station_fryer_jp (đã flood-fill nền hồng viền thành trong suốt), drink -> station_drink_jp, pantry -> storage_shelf_2_red hedge (thay drawShelf, giữ vòng tiến độ).
- Tường: poster overview v2 (restaurant_full_game_red hedge) vẽ trên tường sau bằng onFace trong drawWalls (30_world.js), chỉ khi ảnh đã load.
- Loading: assets/minis/loading_bg.jpg đổi thành ảnh P1_full_red hedge (JPG, bản cũ lưu ở /tmp/loading_bg_old.jpg, không nằm trong game).
- Ảnh v2 không vào game dưới dạng nguyên tấm: overview/P1 scene toàn cảnh vẫn không thể làm nền sàn game (game vẽ sàn/tường/bàn bằng code iso, ảnh toàn cảnh là phối cảnh khác); chúng vào qua poster tường + loading. Counter code vẫn giữ (biển PICK UP nhím đã có từ round 1), không thay bằng ảnh counter_center mặt trước vì sẽ vỡ phân đoạn counter và vị trí khay.

### 6c. legY đo lại theo từng pose (src/16_more_sprites.js)
- Phương pháp: với mỗi file nhím gốc, lấy alpha bbox, đo chiều rộng opaque từng hàng (làm mượt cửa sổ 11), lấy chiều rộng thân ở giữa ảnh (hàng 50% bbox), legY = hàng đầu tiên tính từ chân lên mà độ rộng đạt >=85% chiều rộng thân, rồi kẹp trong [75%, 92%] chiều cao chân (ay) để tránh rung cả thân khi đi. Chân thấp nhất (ay) đo từ hàng opaque cuối cùng của từng pose, không dùng chung một tỷ lệ.
- Kết quả: 224/224 pose đã cập nhật, 0 file thiếu, tỷ lệ legY/ay từ 0,747 đến 0,920, trung bình 0,822 (cũ là 0,84 phẳng theo chiều cao bbox). node --check OK.
- Giới hạn còn lại của phép đo này (nói thẳng): sprite nhím chibi là một khối liền, không có đường khớp hông nhìn thấy được, nên legY vẫn là ước lượng có căn cứ theo profile độ rộng, không phải khớp giải phẫu chính xác; kẹp 75-92% là để an toàn cho animation.

### 6d. Assets PNG còn mèo (89 file thay + 10 file ic/raw)
- Characters: 27 file cat_*.png (strip 8dir: 8 frame idle_0/45/90/135/180/225/270/315 theo đúng thứ tự, mỗi frame fit vào 1/8 chiều rộng strip; file đơn: idle_0 fit) + 32 file staff preview (back=180, front=0, left=90, right=270; walk_right=walkN_270). Ánh xạ vai trò: chef=01, cashier=02, waiter=03, waitress=07, manager=05, vip=10, tourist=08, helper=04, kid=07, c_grey=06, c_ginger=08, c_tux=10, c_calico=07, c_cream=09, c_dark=06, c_white=07, c_elder=05 (có dùng lại vai trò, ghi rõ).
- Restaurant/stations/outdoor/items có đối tác v2 rõ ràng: bin->trash_bin_red hedge (file cũ có dấu chân), ctr_boxes/plant/register/shelf/tray, dc_plant/planter, table/table4p, st_oven/drink/prep/special/veggie/dessert/seafood/pantry, gd_pot/gd_bush, it_drink->cat_cola_red hedge, it_box->chicken_box_red hedge, it_pizza/pepperoni/veggie/seafood/special/dessert/trash/ing, ic_* và raw_* (10 file) -> món v2 tương ứng. Tất cả fit không kéo giãn, giữ nguyên width/height/anchor của file gốc.
- Đã kiểm tra trực tiếp các file không thay: chair_backR, grand, cart_awning, it_ic_pizza, vip, dc_board, st_drink (trước khi thay) — không có mặt mèo/tai/râu/dấu chân; các file còn lại không thay là ghế, rào, biển, cây, xu, emote, cart — đều không có mèo, chỉ là preview code painter. Minis đã là v2 từ round 1 nên không tính lại.
- Sau round 2, grep src không còn CAT_SKINS/drawRigCat/drawEar/catOf/CAT_PIZZA_USE_FILES. Trong dist/index.html: "Red Hedge"=32, các cụm hiển thị "Meow Rush/Cat Cola/cashier cat/cleaner cat/Calling the cats/Ready! Meow/Meow Burger Diner"=0; 5 lần "Meow" còn lại chỉ là window.MeowBurgerAds và comment lịch sử (Meow Sushi/Meow Pizza/ex-Meow Burger). Các chuỗi "Cat" còn lại trong dist chỉ là nhiễu base64 của ảnh, không phải text.

### 6e. Build round 2
- python3 src/build.py /tmp/node_modules/.bin/esbuild: EXIT 0, dist/index.html 11 MB, dist/game.js 8,8 MB, node --check dist/game.js OK, node --check mọi src/*.js OK.
- Bản HTML vẫn chưa được mở kiểm tra trong trình duyệt thật (subagent không có trình duyệt live) — cần parent kiểm tra framing trạm v2, poster tường, rig nhím fallback, và bước chân với legY mới.

## 7. Native TikTok test build (2026-10-25)
- Lệnh: PATH=/tmp/node_modules/.bin:$PATH python3 src/build_native.py, với src/native/hs-config.json là bản copy của hs-config.example.json (placeholder YOUR_TIKTOK_APP_ID / sk_live_...). Vì vậy bản này CHƠI ĐƯỢC nhưng ads/missions/cloud save thật đều tắt — chỉ chạy trong hs-check mô phỏng.
- Vướng môi trường (đã xử lý, không sửa nguồn gốc): build_native lần đầu dừng ở tools/native_icons.js vì thiếu Playwright; Chromium headless trên VM cũng không chụp được ảnh. Đã tạo icon bằng cairosvg (pip install cairosvg) từ đúng UI_ICON/STAGE_ART gốc: 19 ic_* + ic_share + 4 stage_*, đã kiểm tra bằng mắt ic_coin và stage_0 render đúng. Tool gốc đã khôi phục nguyên trạng sau build.
- Kết quả: 269 sprite file, main 2,87 MB (<4 MB), art 22,29 MB, tổng 25,15 MB (<30 MB). game.json ở root dist/native, chứa v2 art (game.js native có V2_STATION_SRC/drawV2Station) và Red Hedge.
- hs-check (vendor/hs-tiktok-sdk-1.0.7/hs-check.mjs dist/native): 0 fail, 15 warn — tất cả đều là placeholder đã biết: appid chưa set, label có chữ hoa (theo README là kỳ vọng), ad id placeholder. PASS: package, SDK v1.0.7, boot không lỗi, SDK start, login, rewarded ad 3s, onGive, dashboard JSON, cloud save/restore mô phỏng.
- Cảnh báo label còn lại: hs-config placeholder vẫn ghi "Meow Rush" trong khi game dùng "Hedge Rush" (build cảnh báo placement thiếu/thừa); dashboard JSON sinh ra vẫn hợp lệ nhưng khi đăng ký thật cần sửa label này thành Hedge Rush và điền ad unit thật.
- Zip: ~/workspace/your_files/red hedge-native-tiktok-test.zip (24 MB, 313 entry, testzip sạch, game.json ở root zip). File dist/native.hs-dashboard.json nằm NGOÀI zip (theo thiết kế build script: chỉ để paste vào HS dashboard, không upload).
- Sửa nhỏ kèm theo: src/build_native.py và dist/native/project.config.json/meow-config.js đổi tên project "Meow Burger" -> "Red Hedge" (chỉ metadata, không đổi code game).
