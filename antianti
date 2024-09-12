import dearpygui.dearpygui as dpg, json, requests, time

dpg.create_context()
dpg.create_viewport(title="by wtfmano", width=800, height=600, vsync=False)
dpg.setup_dearpygui()

cachedStats = []
cachedInfo = []

def onSearchStats():
    nick = dpg.get_value("nick_input")
    
    if (len(nick) == 0):
        return

    if (nick in cachedStats):
        return

    mr = requests.get(f"https://mush.com.br/api/player/{nick}")

    if ("\"error_code\":404" in mr.text):
        with dpg.window(label="error", modal=True):
            dpg.add_text("player not found or using /fake")
        return
    
    if ("first_login" not in mr.text):
        with dpg.window(label="error", modal=True):
            dpg.add_text("player not played on mush")
        return
    
    bwList = json.loads(mr.text)["response"]["stats"]["bedwars"]

    if ("games_played" not in bwList):
        with dpg.window(label="error", modal=True):
            dpg.add_text("player not played bedwars")
        return

    bwList["fkdr"] = int(bwList["final_kills"] / bwList["losses"])

    with dpg.window(label=f"{nick} stats", on_close=lambda: cachedStats.pop(cachedStats.index(nick))):
        cachedStats.append(nick)
        
        with dpg.table(header_row=True, borders_innerH=True, borders_outerH=True,
                   borders_innerV=True, borders_outerV=True, row_background=True):
            
            dpg.add_table_column(label="bedwars", no_sort=True)

            dict.pop(bwList, "level_badge")

            for k in bwList:
                with dpg.table_row():
                    dpg.add_text(f"{str.replace(k, "_", " ")}: {bwList[k]}")

def onPlayerInfo():
    nick = dpg.get_value("nick_input2")

    if (len(nick) == 0):
        return

    if (nick in cachedInfo):
        return

    mr = requests.get(f"https://mush.com.br/api/player/{nick}")

    if ("\"error_code\":404" in mr.text):
        with dpg.window(label="error", modal=True):
            dpg.add_text("player not found or using /fake")
        return
    
    if ("first_login" not in mr.text):
        with dpg.window(label="error", modal=True):
            dpg.add_text("player not played on mush")
        return

    playerInfo = json.loads(mr.text)['response']

    with dpg.window(label=f"{nick} info", on_close=lambda: cachedInfo.pop(cachedInfo.index(nick))):
        cachedInfo.append(nick)

        with dpg.table(header_row=True, borders_innerH=True, borders_outerH=True,
                   borders_innerV=True, borders_outerV=True, row_background=True):
            
            dpg.add_table_column(label="player info", no_sort=True)
            
            tagList = []

            for k in playerInfo["tags"]:
                tagList.append(k)

            infoList = {
                f"account type: {playerInfo["account"]["type"]}",
                f"best tag: {playerInfo["best_tag"]["name"]}",
                f"rank tag: {playerInfo["rank_tag"]["name"]}",
                f"best tag: {playerInfo["best_tag"]["name"]}",
                f"connected: {playerInfo["connected"]}",
                f"first login: {time.strftime('%d/%m/%Y %H:%M:%S', time.gmtime(playerInfo["first_login"] / 1000))}",
                f"last login: {time.strftime('%d/%m/%Y %H:%M:%S', time.gmtime(playerInfo["last_login"] / 1000))}",
                f"tags: {" ".join(tagList).replace(" ", ", ")}",
                f"playtime: {"not played" if dict.get(playerInfo["stats"], "play_time") is None else time.strftime('%dd %Mm %Hh %Ss', time.gmtime(playerInfo["stats"]["play_time"]["all"] / 1000))}",
                f"banned: {"yes" if dict.get(playerInfo, "banned") is not None else "no"}",
                f"muted: {"yes" if dict.get(playerInfo, "muted") is not None else "no"}"
            }

            for rip in infoList:
                with dpg.table_row():
                    dpg.add_text(rip)

width, height, fodase, data = dpg.load_image("neexty_real.png")

with dpg.texture_registry(show=False):
    dpg.add_dynamic_texture(width=width, height=height, default_value=data, tag="neexty_real")

with dpg.window(label="stats info", pos=(10, 0), width=120):
    dpg.add_input_text(tag="nick_input")
    dpg.add_button(label="get stats", tag="gstatsButton", callback=onSearchStats)

with dpg.window(label="player info", pos=(140, 0)):
    dpg.add_input_text(tag="nick_input2")
    dpg.add_button(label="get player info", tag="pinfoButton", callback=onPlayerInfo)

with dpg.window(label="credits", modal=True, pos=(800 / 3, 600 / 6)):
    dpg.add_text("thanks to suqqo for tool idea")
    dpg.add_text("average mushmc moderator, lmfao")
    dpg.add_image("neexty_real")

dpg.show_viewport()
dpg.start_dearpygui()
dpg.destroy_context()
