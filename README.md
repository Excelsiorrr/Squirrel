# Introduction
Personal configurations for `鼠鬚管`, a Mac powerful input method for Chinese characters based on Rime@https://github.com/rime/squirrel


# Prerequisite
* lua-5.4.3
* 鼠须管0.15.2

# Directory
* `~/Library/Rime/`

# Install Step-by-step
1. https://rime.im/ 官网下载安装鼠须管
2. http://www.lua.org/ 官网下载配置好lua
3. 将此repo里所有文件复制/替换到`~/Library/Rime/`
4. reload/重新部署
   
# Core Files
* `./default.custom.yaml`:
  ```
  patch:
    menu/page_size: 9           # 一页显示9个词条
    schema_list:                # 只使用`luna_pinyin`作为主输入法
        - schema: luna_pinyin
  ```

* `./squirrel.custom.yaml`: 
  ```
  patch:
    style/horizontal: true      # 横向
    style/color_scheme: apathy  # 主题使用apathy
    app_options:                # 在切换到以下应用时, 自动切换成英文输入法
        com.microsoft.VSCode:   # 用以下命令来获得app名称
            ascii_mode: true    # cat `/Applications/*.app/Contents/Info.plist" | grep com
        com.googlecode.iterm2:
            ascii_mode: true
        com.runningwithcrayons.Alfred:
            ascii_mode: true
  ```
* `./luna_pinyin.custom.yaml`
  ```
  patch:
    # type "date" or "time"
    engine/translators/@6: lua_translator@date_translator
    engine/translators/@7: lua_translator@time_translator
    'speller/algebra':
        #- erase/^xx$/ 
        - derive/^([zcs])h/$1/             # zh, ch, sh => z, c, s
        - derive/^([zcs])([^h])/$1h$2/     # z, c, s => zh, ch, sh
        # 模糊音定義先於簡拼定義，方可令簡拼支持以上模糊音
        - abbrev/^([a-z]).+$/$1/           # 簡拼（首字母）
        - abbrev/^([zcs]h).+$/$1/          # 簡拼（zh, ch, sh）

        # 以下是一組容錯拼寫，《漢語拼音》方案以前者爲正
        - derive/^([nl])ve$/$1ue/          # nve = nue, lve = lue
        - derive/^([jqxy])u/$1v/           # ju = jv,
        - derive/un$/uen/                  # gun = guen,
        - derive/ui$/uei/                  # gui = guei,
        - derive/iu$/iou/                  # jiu = jiou,

        # 自動糾正一些常見的按鍵錯誤
        - derive/([aeiou])ng$/$1gn/        # dagn => dang 
        - derive/([dtngkhrzcs])o(u|ng)$/$1o/  # zho => zhong|zhou
        - derive/ong$/on/                  # zhonguo => zhong guo
        - derive/ao$/oa/                   # hoa => hao
        - derive/([iu])a(o|ng?)$/a$1$2/    # tain => tian

    translator/enable_user_dict: true       
    translator/dictionary: luna_pinyin.custom # 载入自定义词库, **关键**
  ```
* `./luna_pinyin.custom.dict.yaml`
    ```
    ---
    name: luna_pinyin.custom
    version: "2018.10.05"
    sort: by_weight
    use_preset_vocabulary: true
    import_tables:
        #- luna_pinyin.extended # include next 4 dict
        - luna_pinyin
        #- luna_pinyin.hanyu
        - luna_pinyin.poetry
        - luna_pinyin.cn_en
        - luna_pinyin.xiandaihanyuchangyongcibiao
        - luna_pinyin.tanzi_math
        - luna_pinyin.tanzi_midmath
        - luna_pinyin.computer
        - luna_pinyin.english
        #- luna_pinyin.tanzi_en
        #- luna_pinyin.chengyusuyu
        #- luna_pinyin.sijixingzhenquhuadimingciku
        #- luna_pinyin.jisuanjicihuidaquan
        #- luna_pinyin.wangluoliuxingxinci
        #- luna_pinyin.kaifadashenzhuanyongciku
        #- luna_pinyin.shanghaishichengshixinxijingxuan
        #- luna_pinyin.zhongguolishicihuidaquan
        #- luna_pinyin.shanghaihuadaquan
        #- luna_pinyin.mingxing
        - luna_pinyin.emoji
        - luna_pinyin.emoji.apple
        - luna_pinyin.emoji.cldr
        - luna_pinyin.emoji.scomper
    ```
# Reference
* https://github.com/alswl/Rime
* https://github.com/rime/squirrel