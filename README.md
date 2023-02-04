# MusinessBanager




--[[ https://imgur.com/a/kchuhXW

Credits:
- https://www.unknowncheats.me/forum/3337151-post1560.html  <3 boredom1234

Script created by ICYPhoenix#0727 and Ren#5219
]]
--util.require_natives(1651208000)
local IS_RELEASE_VERSION <const> = false
local IS_BETA_VERSION <const> = false
local IGNORE_VERSION_DIFFERENCE <const> = false
local THIS_RELEASE_VERSION <const> = "1.0.0"
local STAND_RESOURCE_DIR = filesystem.resources_dir()
local MB_RESOUCES_DIR = STAND_RESOURCE_DIR .. "Musiness Banager/"
local MB_TRANSLATIONS_DIR = MB_RESOUCES_DIR .. "Translations/"
local MBPrefix = "[MusinessBanager] "
local og_toast = util.toast
local og_log = util.log
local nullsub = function() --[[util.toast("nullsub")]] end
util.toast = function(str, flag) assert(str != nil, "No string given") if flag ~= nil then og_toast(MBPrefix .. tostring(str), flag) else og_toast(MBPrefix .. tostring(str)) end end
util.log = function(str) assert(str != nil, "No string given") og_log(MBPrefix .. tostring(str)) end
util.yield_x = function(int) for i = 1, int do util.yield_once() end end -- yields x amount of ticks
local menu, players, entities, directx, util, v3, lang, filesystem, async_http, memory = menu, players, entities, directx, util, v3, lang, filesystem, async_http, memory

--#region natives
-- regex for removing comments in the native arguments: --\[\[(?:(?:\w)|(?:\d)|\*)*(?: \((?:(?:\w)|(?:\d)|\*)*\))*\]\] 
-- regex for finding natives in the script that have not yet been converted to local-natives: [A-Z][A-Z][A-Z]\.(?:_|[A-Z][A-Z][A-Z][A-Z][A-Z])
local nv = native_invoker
local ENTITY_SET_ENTITY_COORDS_NO_OFFSET                                = function(entity,xPos,yPos,zPos,xAxis,yAxis,zAxis)nv.begin_call();nv.push_arg_int(entity);nv.push_arg_float(xPos);nv.push_arg_float(yPos);nv.push_arg_float(zPos);nv.push_arg_bool(xAxis);nv.push_arg_bool(yAxis);nv.push_arg_bool(zAxis);nv.end_call("239A3351AC1DA385");end
local ENTITY_GET_ENTITY_COORDS                                          = function(entity,alive)nv.begin_call();nv.push_arg_int(entity);nv.push_arg_bool(alive);nv.end_call("3FEF770D40960D5A");return nv.get_return_value_vector3();end
local ENTITY_FREEZE_ENTITY_POSITION                                     = function(entity,toggle)nv.begin_call();nv.push_arg_int(entity);nv.push_arg_bool(toggle);nv.end_call("428CA6DBD1094446");end
local CAM_CREATE_CAM_WITH_PARAMS                                        = function(camName,posX,posY,posZ,rotX,rotY,rotZ,fov,p8,p9)nv.begin_call();nv.push_arg_string(camName);nv.push_arg_float(posX);nv.push_arg_float(posY);nv.push_arg_float(posZ);nv.push_arg_float(rotX);nv.push_arg_float(rotY);nv.push_arg_float(rotZ);nv.push_arg_float(fov);nv.push_arg_bool(p8);nv.push_arg_int(p9);nv.end_call("B51194800B257161");return nv.get_return_value_int();end
local CAM_DESTROY_CAM                                                   = function(cam,bScriptHostCam)nv.begin_call();nv.push_arg_int(cam);nv.push_arg_bool(bScriptHostCam);nv.end_call("865908C81A2C22E9");end
local CAM_GET_FINAL_RENDERED_CAM_FOV                                    = function()nv.begin_call();nv.end_call("80EC114669DAEFF4");return nv.get_return_value_float();end
local CAM_GET_FINAL_RENDERED_CAM_ROT                                    = function(rotationOrder)nv.begin_call();nv.push_arg_int(rotationOrder);nv.end_call("5B4E4C817FCC2DFB");return nv.get_return_value_vector3();end
local CAM_SET_CAM_ACTIVE                                                = function(cam,active)nv.begin_call();nv.push_arg_int(cam);nv.push_arg_bool(active);nv.end_call("026FB97D0A425F84");end
local CAM_RENDER_SCRIPT_CAMS                                            = function(render,ease,easeTime,p3,p4,p5)nv.begin_call();nv.push_arg_bool(render);nv.push_arg_bool(ease);nv.push_arg_int(easeTime);nv.push_arg_bool(p3);nv.push_arg_bool(p4);nv.push_arg_int(p5);nv.end_call("07E5B515DB0636FC");end
local GRAPHICS_TOGGLE_PAUSED_RENDERPHASES                               = function(toggle)nv.begin_call();nv.push_arg_bool(toggle);nv.end_call("DFC252D8A3E15AB7");end
local GRAPHICS_DONT_RENDER_IN_GAME_UI                                   = function(p0)nv.begin_call();nv.push_arg_bool(p0);nv.end_call("22A249A53034450A");end
local PAD_SET_CONTROL_VALUE_NEXT_FRAME                                  = function(padIndex,control,amount)nv.begin_call();nv.push_arg_int(padIndex);nv.push_arg_int(control);nv.push_arg_float(amount);nv.end_call("E8A25867FBA3B05E");return nv.get_return_value_bool();end
local PAD_SET_CURSOR_POSITION                                           = function(x,y)nv.begin_call();nv.push_arg_float(x);nv.push_arg_float(y);nv.end_call("FC695459D4D0E219");return nv.get_return_value_bool();end
local PLAYER_PLAYER_PED_ID                                              = function()nv.begin_call();nv.end_call("D80958FC74E988A6");return nv.get_return_value_int();end
local SYSTEM_START_NEW_SCRIPT                                           = function(scriptName,stackSize)nv.begin_call();nv.push_arg_string(scriptName);nv.push_arg_int(stackSize);nv.end_call("E81651AD79516E48");return nv.get_return_value_int();end
local SCRIPT_GET_NUMBER_OF_THREADS_RUNNING_THE_SCRIPT_WITH_THIS_HASH    = function(scriptHash)nv.begin_call();nv.push_arg_int(scriptHash);nv.end_call("2C83A9DA6BFFC4F9");return nv.get_return_value_int();end
local SCRIPT_DOES_SCRIPT_EXIST                                          = function(scriptName)nv.begin_call();nv.push_arg_string(scriptName);nv.end_call("FC04745FBE67C19A");return nv.get_return_value_bool();end
local SCRIPT_REQUEST_SCRIPT                                             = function(scriptName)nv.begin_call();nv.push_arg_string(scriptName);nv.end_call("6EB5F71AA68F2E8E");end
local SCRIPT_HAS_SCRIPT_LOADED                                          = function(scriptName)nv.begin_call();nv.push_arg_string(scriptName);nv.end_call("E6CC9F3BA0FB9EF1");return nv.get_return_value_bool();end
local SCRIPT_SET_SCRIPT_AS_NO_LONGER_NEEDED                             = function(scriptName)nv.begin_call();nv.push_arg_string(scriptName);nv.end_call("C90D2DCACD56184C");end
local STATS_STAT_GET_INT                                                = function(statHash,outValue,p2)nv.begin_call();nv.push_arg_int(statHash);nv.push_arg_pointer(outValue);nv.push_arg_int(p2);nv.end_call("767FBC2AC802EF3D");return nv.get_return_value_bool();end
local STATS_STAT_SET_INT                                                = function(statName,value,save)nv.begin_call();nv.push_arg_int(statName);nv.push_arg_int(value);nv.push_arg_bool(save);nv.end_call("B3271D7AB655B441");return nv.get_return_value_bool();end
local STATS_STAT_SET_BOOL                                               = function(statName,value,save)native_invoker.begin_call();native_invoker.push_arg_int(statName);native_invoker.push_arg_bool(value);native_invoker.push_arg_bool(save);native_invoker.end_call("4B33C4243DE0C432");return native_invoker.get_return_value_bool();end
local STATS_STAT_GET_BOOL                                               = function(statHash,outValue,p2)native_invoker.begin_call();native_invoker.push_arg_int(statHash);native_invoker.push_arg_pointer(outValue);native_invoker.push_arg_int(p2);native_invoker.end_call("11B5E6D2AE73F48E");return native_invoker.get_return_value_bool();end
local STATS_SET_PACKED_STAT_BOOL_CODE                                   = function(index,value,characterSlot)native_invoker.begin_call();native_invoker.push_arg_int(index);native_invoker.push_arg_bool(value);native_invoker.push_arg_int(characterSlot);native_invoker.end_call("DB8A58AEAA67CD07");end
local STATS_GET_PACKED_STAT_BOOL_CODE                                   = function(index,characterSlot)native_invoker.begin_call();native_invoker.push_arg_int(index);native_invoker.push_arg_int(characterSlot);native_invoker.end_call("DA7EBFC49AE3F1B0");return native_invoker.get_return_value_bool();end
local NETSHOPPING_NET_GAMESERVER_TRANSACTION_IN_PROGRESS                = function()native_invoker.begin_call()native_invoker.end_call_2(0x613F125BA3BD2EB9)return native_invoker.get_return_value_bool();end
local TERMINATE_ALL_SCRIPTS_WITH_THIS_NAME                              = function(scriptName)native_invoker.begin_call()native_invoker.push_arg_string(scriptName)native_invoker.end_call_2(0x9DC711BC69C548DF)end

--#endregion natives

local MenuLabels = {
    BUNKER="Bunker",
    BUSINESS="Business",
    MCBUSINESS="MC Business",
    WAREHOUSE="Warehouse",
    NIGHTCLUB="Nightclub",
    NIGHTCLUBSAFE="Nightclub Safe",
    SPECIALCARGO="Special Cargo",
    SELLMISSION="Sell Mission",
    BUYMISSION="Buy Mission",
    START="Start",
    MONEY="Money",
    STOCK="Stock",
    SUPPLIES="Supplies",
    CRATES="Crates",
    PRODUCT="Product",
    INFOOVERLAY="Info Overlay",
    TERRORBYTE="Terrorbyte",
    --! STOCK, SUPPLIES, PRODUCT should all be what the game calls it, or something equivalent
    --! You might want to leave TERRORBYTE the same, unless the game calls it something different

    HTTPGIVEUP="Failed to establish connection to remote.",
    HTTPINVALID="Received an invalid response from remote! This may be due to Cloudflare or your antivirus blocking the connection. Please either establish a new VPN connection, and or switch networks.",
    HTTPFAILSAFE="Activating Killswitches due to failsafe.",
    SCRIPTOUTOFDATE="Script is out of date! Please restart the script to get the latest from the repository!",

    KILLSWITCH_SAFELOOP="Killing Nightclub Safe AFK Money Loop due to killswitch",
    KILLSWITCH_SPECIALCARGO="Killing Special Cargo due to killswitch",
    KILLSWITCH_MAXSELLPRICE="Killing Max Sell Price due to killswitch",
    KILLSWITCH_AUTOCOMPLETE="Killing Auto Complete due to killswitch",

    TRANSACTIONSSTUCK_TOAST="It seems that your transactions are stuck. Please switch sessions or restart the game.",
    BEALONE_TOAST="There are too many players in your lobby! Triggering bealone",

    PREFIX_SAFELOOP="[Safe Loop] {1}",
    PREFIX_SPECIALCARGO="[Special Cargo] {1}",
    PREFIX_TOTALEARNED="Total Earned: {1}",
    PREFIX_MOTD="MOTD: {1}",
    --! {1} is a number, the amount of money earned in total.
    PREFIX="{1} {2}",
    --! This can be '[x1] x2' where x1 is the prefix, and x2 is the message. This might go unused though..
... (65 KB left)
