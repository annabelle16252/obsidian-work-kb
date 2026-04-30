---
title: "gi2mic_platform.c (revision 1316144) - OpenGrok cross reference for /RELEASE_232_THROTTLE/pfe/common/drivers/mic/gi2mic_platform.c"
source: "https://opengrok.juniper.net/source/xref/RELEASE_232_THROTTLE/pfe/common/drivers/mic/gi2mic_platform.c?r=1316144&mo=30987&fi=1060#1060"
author:
published:
created: 2026-04-30
description:
tags:
  - "clippings"
---
# gi2mic_platform.c (revision 1316144) - OpenGrok cross reference for /RELEASE_232_THROTTLE/pfe/common/drivers/mic/gi2mic_platform.c

```
/* <![CDATA[ */
function get_sym_list(){return [["Macro","xm",[["GMIC_RC",137],["MACSEC_INVALID_VALUE",68]]],["Variable","xv",[["gi2mic_ge_mode",143],["gi2mic_mdio_lock",144],["gi2mic_vsc8490_1g_port_map",87],["gi2mic_vsc8584_1g_port_map",93],["mic_gi2_smic_mac_vectors",2192],["mic_gi2_smic_phy_vectors",2446],["mic_gi2_xge_phy_vector",2530],["vsc8490_bd_thread_start_time",72],["vsc8490_bounded_delay_interface_count",70],["vsc8490_bounded_delay_interface_list",71],["vsc8490_macsec_tx_lapn_thread_created",69],["vsc8584_bd_thread_start_time",76],["vsc8584_bounded_delay_interface_count",74],["vsc8584_bounded_delay_interface_list",75],["vsc8584_macsec_tx_lapn_thread_created",73],["vsc8584_reset_conf",127],["vtss_appl_trace_conf",145]]],["Struct","xs",[["gi2mic_port_map_s",78]]],["Typedef","xt",[["gi2mic_port_map_t",85]]],["Function","xf",[["gi2mic_1g_link_init",3791],["gi2mic_1g_link_init_all",3861],["gi2mic_cl37_mdio_reg_read",338],["gi2mic_cl37_mdio_reg_write",396],["gi2mic_cl37_reg_read",449],["gi2mic_cl37_reg_write",467],["gi2mic_cl45_mdio_reg_read",1451],["gi2mic_cl45_mdio_reg_write",1514],["gi2mic_cl45_reg_read",1646],["gi2mic_cl45_reg_read_inc",1665],["gi2mic_cl45_reg_write",1675],["gi2mic_create",3756],["gi2mic_get_macsec_ifl_stats",2966],["gi2mic_get_macsec_stats",2983],["gi2mic_get_vsc8490_instance",1434],["gi2mic_get_vsc8490_itable_index",1440],["gi2mic_get_vsc8490_obj",1398],["gi2mic_get_vsc8490_port",1419],["gi2mic_get_vsc8584_instance",292],["gi2mic_get_vsc8584_itable_index",298],["gi2mic_get_vsc8584_obj",312],["gi2mic_get_vsc8584_port",304],["gi2mic_lock_create",263],["gi2mic_lock_destroy",275],["gi2mic_macsec_block_disable",2950],["gi2mic_macsec_block_enable",2933],["gi2mic_macsec_config_vectors",3678],["gi2mic_macsec_control_flow_create",3118],["gi2mic_macsec_control_flow_delete",3154],["gi2mic_macsec_event_handler",3014],["gi2mic_macsec_flow_delete",2917],["gi2mic_macsec_ifl_supported",3136],["gi2mic_macsec_init",2786],["gi2mic_macsec_init_post_issu",3148],["gi2mic_macsec_port_reinit",2804],["gi2mic_macsec_rx_activate",3068],["gi2mic_macsec_rx_flow_create",2901],["gi2mic_macsec_rx_sa_create",3042],["gi2mic_macsec_rx_sa_lowest_pn_update",3171],["gi2mic_macsec_rx_sc_create",2833],["gi2mic_macsec_rx_sc_delete",3107],["gi2mic_macsec_sa_create",2865],["gi2mic_macsec_sc_delete",2849],["gi2mic_macsec_secy_create",3020],["gi2mic_macsec_secy_delete",3085],["gi2mic_macsec_tx_activate",3053],["gi2mic_macsec_tx_flow_create",2885],["gi2mic_macsec_tx_sa_create",3031],["gi2mic_macsec_tx_sa_lowest_pn_poll",3655],["gi2mic_macsec_tx_sc_create",2817],["gi2mic_macsec_tx_sc_delete",3096],["gi2mic_macsec_update_addr",2999],["gi2mic_miim_reg_read",486],["gi2mic_miim_reg_write",497],["gi2mic_mutex_create",149],["gi2mic_mutex_destroy",166],["gi2mic_mutex_give",247],["gi2mic_mutex_take",175],["gi2mic_obj_create",3733],["gi2mic_periodic_access_sanity_check",2711],["gi2mic_phy_mdio_periodic_access_test",2557],["gi2mic_platform_init",3779],["gi2mic_vsc8490_instance_init",1693],["gi2mic_vsc8490_macsec_tx_sa_lowest_pn_poll",3262],["gi2mic_vsc8490_mmd_read",1580],["gi2mic_vsc8490_mmd_read_inc",1600],["gi2mic_vsc8490_mmd_write",1625],["gi2mic_vsc8490_port_init",1736],["gi2mic_vsc8584_an_get",1339],["gi2mic_vsc8584_an_init",1260],["gi2mic_vsc8584_an_restart",1319],["gi2mic_vsc8584_an_set",1303],["gi2mic_vsc8584_dev_reset",1003],["gi2mic_vsc8584_duplex_get",1233],["gi2mic_vsc8584_duplex_set",1217],["gi2mic_vsc8584_enable_set",1015],["gi2mic_vsc8584_get_link_intr_status",1364],["gi2mic_vsc8584_init",901],["gi2mic_vsc8584_instance_create",756],["gi2mic_vsc8584_instance_destroy",776],["gi2mic_vsc8584_instance_init",508],["gi2mic_vsc8584_link_intr_enable",1385],["gi2mic_vsc8584_link_status_get",1060],["gi2mic_vsc8584_local_loopback_set",1028],["gi2mic_vsc8584_macsec_tx_sa_lowest_pn_poll",3496],["gi2mic_vsc8584_obj_init",801],["gi2mic_vsc8584_phy_config",547],["gi2mic_vsc8584_phy_init",670],["gi2mic_vsc8584_port_init",935],["gi2mic_vsc8584_remote_loopback_set",1042],["gi2mic_vsc8584_speed_get",1157],["gi2mic_vsc8584_speed_set",1189],["gi2mic_vsc8584_update_lpinfo",1252],["gmic2_service_thread_entry",3894],["mic_gi2_get_lnk_phy_vectors",2458],["mic_gi2_get_xge_phy_vector",2542],["mic_gi2_lnk_phy_create",2217],["mic_gi2_lnk_phy_destroy",2257],["mic_gi2_lnk_phy_dev_init",2297],["mic_gi2_lnk_phy_ioctl",2340],["mic_gi2_lnk_phy_periodic",2400],["mic_gi2_mac_get_lnk_vectors",2202],["mic_gi2_mac_lnk_create",1922],["mic_gi2_mac_lnk_destroy",1975],["mic_gi2_mac_lnk_dev_init",2028],["mic_gi2_mac_lnk_ioctl",2081],["mic_gi2_mac_lnk_periodic",2135],["mic_gi2mic_lnk_ioctl",2469],["mic_gi2mic_lnk_periodic",2518],["vsc8490_macsec_tx_lapn_thread_fn",3190],["vsc8584_macsec_tx_lapn_thread_fn",3424]]]];} /* ]]> */1 /*
2  * $Id: gi2mic_platform.c 933147 2018-04-14 05:34:51Z swaathiv $
3  *
4  * gi2mic_platform.c : Goose Island Phase II MIC, platform file
5  *
6  * --sundarpv@juniper.net, April 2018
7  *
8  * Copyright (c) 2017-2018, Juniper Networks, Inc.
9  * All rights reserved.
10  */
11 
12 #include "defs.h"
13 #include "malloc.h"
14 #include "stdio.h"
15 #include "thread.h"
16 #include "syslog.h"
17 #include "syslog/priv_syslog.h"
18 #include "interrupt.h"
19 #include "if.h"
20 #include <wakeup.h>
21 #include <timers.h>
22 #include "time.h"
23 #include "i2c_shared.h"
24 
25 #include "ipc.h"
26 #include "ipc_if.h"
27 #include "ipc_pos.h"
28 #include "ipc_port.h"
29 #include "ipc_ge.h"
30 #include "gether.h"
31 #include "autoneg/autoneg.h"
32 
33 #include "cmlc/cmlc.h"
34 #include "lnk/lnk.h"
35 
36 #include "i2c_api.h"
37 #include "priv_mic.h"
38 #include "priv_mic_as.h"
39 #include "mic_an.h"
40 #include "mic_issu.h"
41 #include "mic_phy.h"
42 #include "priv_gi2mic.h"
43 #include "priv_mic_si53xx.h"
44 #include "priv_mic_i2cs.h"
45 #include "mqchip/mqchip.h"
46 #include <center-chip/center_chip.h>
47 #include "cmt/cmtfpc.h"
48 #include "ixchip/ix.h"
49 #include "pio.h"
50 #include "pci_pio.h"
51 #include "vsc8584/vsc8584.h"
52 #include "vsc8490/vsc8490.h"
53 #include "vsc8584/priv_vsc8584_macsec.h"
54 #include "vsc8490/priv_vsc8490_macsec.h"
55 #include "i2c-acceleration/i2c_accel_sl.h"
56 #include "pca9551/pca9551.h"
57 #include "max6581/max6581.h"
58 #include "smart-sfp/smart_sfp.h"
59 #include "priv_gi2mic.h"
60 #include "vtss_phy_10g_api.h"
61 #include "vsc8584/priv_vsc8584.h"
62 #include "vsc8490/priv_vsc8490.h"
63 #include "vtss_phy_api.h"
64 #include "pic.h"
65 #include "issu.h"
66 
67 
68 #define MACSEC_INVALID_VALUE 0xDEADBEEF
69 static boolean vsc8490_macsec_tx_lapn_thread_created = FALSE;
70 static int vsc8490_bounded_delay_interface_count = 0;
71 vsc8490_macsec_bounded_delay_list_t vsc8490_bounded_delay_interface_list;
72 time_ms_t vsc8490_bd_thread_start_time;
73 static boolean vsc8584_macsec_tx_lapn_thread_created = FALSE;
74 static int vsc8584_bounded_delay_interface_count = 0;
75 vsc8584_macsec_bounded_delay_list_t vsc8584_bounded_delay_interface_list;
76 time_ms_t vsc8584_bd_thread_start_time;
77 
78 typedef struct gi2mic_port_map_s {
79     int index;
80     int phy_addr;
81     int base_phy_addr;
82     int internal_port_no;
83     int pre_reset_enable;
84     int post_reset_enable;
85 } gi2mic_port_map_t;
86 
87 gi2mic_port_map_t gi2mic_vsc8490_1g_port_map[] = {
88     /* PHY 6 */
89     { 0, 20, 20, 0, 1, 0 },
90     { 1, 21, 21, 1, 0, 1 },
91 };
92 
93 gi2mic_port_map_t gi2mic_vsc8584_1g_port_map[] = {
94     /* PHY 1 */
95     { 0, 0, 0, 0, 1, 0 },
96     { 1, 1, 0, 1, 0, 0 },
97     { 2, 2, 0, 2, 0, 0 },
98     { 3, 3, 0, 3, 0, 1 },
99 
100     /* PHY 2 */
101     { 4, 4, 4, 0, 1, 0 },
102     { 5, 5, 4, 1, 0, 0 },
103     { 6, 6, 4, 2, 0, 0 },
104     { 7, 7, 4, 3, 0, 1 },
105 
106     /* PHY 3 */
107     { 8, 8, 8, 0, 1, 0 },
108     { 9, 9, 8, 1, 0, 0 },
109     { 10, 10, 8, 2, 0, 0 },
110     { 11, 11, 8, 3, 0, 1 },
111 
112     /* PHY 4 */
113     { 12, 12, 12, 0, 1, 0 },
114     { 13, 13, 12, 1, 0, 0 },
115     { 14, 14, 12, 2, 0, 0 },
116     { 15, 15, 12, 3, 0, 1 },
117 
118     /* PHY 5 */
119     { 16, 16, 16, 0, 1, 0 },
120     { 17, 17, 16, 1, 0, 1 },
121 
122     /* PHY 6 */
123     { 18, 20, 20, 0, 0, 0 },
124     { 19, 21, 20, 1, 0, 0 },
125 };
126 
127 vtss_phy_reset_conf_t vsc8584_reset_conf = {
128     .mac_if  = VTSS_PORT_INTERFACE_SGMII,
129     .media_if  = VTSS_PHY_MEDIA_IF_FI_1000BX,
130     .rgmii = {0},
131     .tbi = {FALSE},
132     .force = 1,
133     .pkt_mode = VTSS_PHY_PKT_MODE_JUMBO_9_KB,
134     .i_cpu_en = 0,
135 };
136 
137 #define GMIC_RC(x)                 \
138     do {                           \
139        if (x != EOK)               \
140           return rc;               \
141     } while(0)
142 
143 u_int8_t gi2mic_ge_mode[MAX_MICS_PER_FPC] = {0xff, 0xff, 0xff, 0xff};
144 sal_mutex_t gi2mic_mdio_lock[MAX_MICS_PER_FPC];
145 static vtss_trace_conf_t vtss_appl_trace_conf;
146 extern u_int32_t   vsc8584_stats_periodic_count;
147 extern u_int32_t   vsc8490_stats_periodic_count;
148 sal_mutex_t
gi2mic_mutex_create(char * desc)149 gi2mic_mutex_create (char *desc)
150 {
151     recursive_mutex_t   *rm;
152 
153     if ((rm = sal_alloc(sizeof (recursive_mutex_t), desc)) == NULL) {
154         return NULL;
155     }
156 
157     rm->mutex = mutex_alloc();
158     rm->owner = 0;
159     rm->recurse_count = 0;
160     rm->desc = desc;
161 
162     return (sal_mutex_t) rm;
163 }
164 
165 void
gi2mic_mutex_destroy(sal_mutex_t m)166 gi2mic_mutex_destroy (sal_mutex_t m)
167 {
168     recursive_mutex_t   *rm = (recursive_mutex_t *) m;
169 
170     mutex_free(rm->mutex);
171     sal_free(rm);
172 }
173 
174 int
gi2mic_mutex_take(sal_mutex_t m,int usec)175 gi2mic_mutex_take (sal_mutex_t m, int usec)
176 {
177     recursive_mutex_t    *rm = (recursive_mutex_t *) m;
178     sal_thread_t    myself = sal_thread_self();
179     int                err = -1;
180     int             status;
181     wakeup_trigger_t *wt;
182 
183     if (rm->owner == myself) {
184         rm->recurse_count++;
185         return 0;
186     }
187 
188     /*
189      * Be optimistic and try to grab mutex without sleeping.
190      */
191     status = mutex_lock(rm->mutex, MUTEX_LOCK_WRITE);
192     if (status == EOK) {
193         err = 0;
194     } else {
195         if (usec == sal_sem_FOREVER) {
196             /* We didn't succeed, so we may have to sleep on the mutex. */
197             wt = mutex_trigger_get(rm->mutex, thread_get_current_pid());
198             if (wt == NULL){
199                 wt = mutex_trigger_create(rm->mutex,
200                                           thread_get_current_pid(), WAKEUP_ID_UNKNOWN);
201                 if (wt == NULL){
202                     return -1;
203                 }
204             }
205             err = mutex_lock_wait(rm->mutex, MUTEX_LOCK_WRITE, NULL);
206         } else {
207             int     time_wait = 1;
208 
209             /*
210              * Retry algorithm with exponential backoff
211              */
212             while (1) {
213                 status = mutex_lock(rm->mutex, MUTEX_LOCK_WRITE);
214                 if (status == 0) {
215                     err = 0;
216                     break;
217                 }
218 
219                 if (time_wait > usec) {
220                     time_wait = usec;
221                 }
222 
223                 sal_usleep(time_wait);
224 
225                 usec -= time_wait;
226 
227                 if (usec == 0) {
228                     err = -1;
229                     break;
230                 }
231 
232                 if ((time_wait *= 2) > 100000) {
233                     time_wait = 100000;
234                 }
235             }
236         }
237     }
238 
239 
240     if (!err) {
241         rm->owner = myself;
242     }
243     return err;
244 }
245 
246 int
gi2mic_mutex_give(sal_mutex_t m)247 gi2mic_mutex_give (sal_mutex_t m)
248 {
249 
250     recursive_mutex_t   *rm = (recursive_mutex_t *) m;
251 
252     if (rm->recurse_count > 0) {
253         rm->recurse_count--;
254         return 0;
255     }
256     rm->owner = 0;
257     mutex_unlock(rm->mutex, MUTEX_LOCK_WRITE);
258     return 0;
259 }
260 
261 
262 sal_mutex_t
gi2mic_lock_create(int slot)263 gi2mic_lock_create(int slot)
264 {
265     if (!gi2mic_mdio_lock[slot]) {
266         gi2mic_mdio_lock[slot] = gi2mic_mutex_create("GI2MIC MDIO Mutex");
267         if (gi2mic_mdio_lock[slot] == NULL)
268             return NULL;
269     }
270 
271     return gi2mic_mdio_lock[slot];
272 }
273 
274 void
gi2mic_lock_destroy(int slot)275 gi2mic_lock_destroy(int slot)
276 {
277     if (gi2mic_mdio_lock[slot]) {
278         gi2mic_mutex_destroy(gi2mic_mdio_lock[slot]);
279         gi2mic_mdio_lock[slot] = 0;
280     }
281 }
282 
283 #ifndef VSC8584
284 /*
285  * gi2mic_get_vsc8584_instance:
286  *
287  * typically the instances are managed 1 instance per mic
288  * MX104 is an exception, where the 4 MICs are supported across 2 virtual FPs
289  * in an AFEB. hence, there will be 4 instances supported across 2 FPCs catering to 2 MICs
290  */
291 int
gi2mic_get_vsc8584_instance(vsc8584_phy_t * obj)292 gi2mic_get_vsc8584_instance(vsc8584_phy_t *obj)
293 {
294     return ((obj->fpc_slot%2)*2 + (obj->mic_slot%2));
295 }
296 
297 int
gi2mic_get_vsc8584_itable_index(ifd_t * ifd __unused)298 gi2mic_get_vsc8584_itable_index(ifd_t *ifd __unused)
299 {
300     return 0;
301 }
302 
303 int
gi2mic_get_vsc8584_port(vsc8584_phy_t * obj)304 gi2mic_get_vsc8584_port(vsc8584_phy_t *obj)
305 {
306     CHECK_NULLOBJ;
307 
308     return (obj->phy_addr);
309 }
310 
311 void *
gi2mic_get_vsc8584_obj(vtss_inst_t inst,u_int16_t port)312 gi2mic_get_vsc8584_obj (vtss_inst_t inst, u_int16_t port)
313 {
314     vsc8584_phy_t *obj = NULL;
315     index16_t temp_index = 1;
316 
317     /* index 1 is a starting index for vsc8490 */
318     for (temp_index = 1; temp_index < vsc8584_ctrl.itable->itable16_maxentry; temp_index++) {
319         obj = vsc8584_itable16_lookup(temp_index);
320         if (obj) {
321             if (((obj->inst) == inst) &&
322                  (VSC8584_PORT == port)) {
323                 return obj;
324             }
325         }
326     }
327 
328     /* Not found after scanning all entries */
329     return NULL;
330 }
331 
332 /*
333  * gi2mic_cl37_mdio_reg_read
334  * This function reads the vsc8584 phy using the registered
335  * mdio access method
336  */
337 status_t
gi2mic_cl37_mdio_reg_read(vsc8584_phy_t * obj,u_int16_t reg,u_int16_t * data)338 gi2mic_cl37_mdio_reg_read (vsc8584_phy_t *obj, u_int16_t reg, u_int16_t *data)
339 {
340     status_t           res = 0;
341     lnk_mdio_vector_t *mdio_func = NULL;
342     ifd_t             *ifd = NULL;
343     u_int8_t           phy_addr = 0;
344     level_t            level = 0;
345 
346     CHECK_NULLOBJ;
347 
348     mdio_func = obj->mdio_func;
349     ifd       = obj->ifd;
350     phy_addr  = obj->phy_addr;
351 
352 #ifdef VSC8584_DEBUG
353     GI2MIC_LOG(LOG_DEBUG,"%s: inst:%p, port = %d phy addr = 0x%x \n"
354                __FUNCTION__,obj->inst, obj->link, obj->phy_addr);
355 #endif
356 
357     /*
358      * The MDIO is shared by all the ports/devices on the chip.
359      * So the MDIO operation needs to be interrupt protected.
360      * Raise interrupts. Interrupts are restored within 150usec.
361      */
362     level = interrupt_raise_priority(LEVEL_NETS);
363 
364     if ((res = mdio_func->mdio_mii_op( ifd,
365                     phy_addr,
366                     reg,
367                     LNK_MDIO_MII_READ,
368                     LNK_MDIO_BUSY_WAIT_UNTIL_DONE,
369                     data)) != EOK) {
370         interrupt_restore_priority(level);
371         GI2MIC_LOG(LOG_ERR, "%s - Error (%d) to read register: phy_addr 0x%x, "
372                    "reg 0x%x\n", __FUNCTION__, res, phy_addr, reg);
373 
374         return res;
375     }
376 
377     /*
378      * Restore interrupts
379      */
380     interrupt_restore_priority(level);
381 
382 #ifdef VSC8584_DEBUG
383     GI2MIC_LOG(LOG_DEBUG, "%s: read register: phy_addr 0x%x,reg 0x%x, data 0x%x\n",
384                __FUNCTION__, phy_addr, reg, *data);
385 #endif
386 
387     return EOK;
388 }
389 
390 /*
391  * gi2mic_cl37_mdio_reg_write
392  * This function writes to vsc8584 phy using the registered
393  * mdio access method
394  */
395 status_t
gi2mic_cl37_mdio_reg_write(vsc8584_phy_t * obj,u_int16_t reg,u_int16_t * data)396 gi2mic_cl37_mdio_reg_write (vsc8584_phy_t *obj, u_int16_t reg, u_int16_t * data)
397 {
398     status_t            res;
399     lnk_mdio_vector_t  *mdio_func;
400     ifd_t              *ifd;
401     u_int8_t            phy_addr;
402     level_t            level;
403 
404     CHECK_NULLOBJ;
405 
406     mdio_func = obj->mdio_func;
407     ifd       = obj->ifd;
408     phy_addr  = obj->phy_addr;
409 
410 #ifdef VSC8584_DEBUG
411     GI2MIC_LOG(LOG_DEBUG,"%s: inst:%p, port = %d phy addr = 0x%x\n",
412                __FUNCTION__,obj->inst, obj->phy_addr,phy_addr);
413 #endif
414 
415     /*
416      * The MDIO is shared by all the ports/devices on the chip.
417      * So the MDIO operation needs to be interrupt protected.
418      * Raise interrupts. Interrupts are restored within 150usec.
419      */
420     level = interrupt_raise_priority(LEVEL_NETS);
421 
422     if ((res = mdio_func->mdio_mii_op( ifd,
423                     phy_addr,
424                     reg,
425                     LNK_MDIO_MII_WRITE,
426                     LNK_MDIO_BUSY_WAIT_UNTIL_DONE,
427                     data)) != EOK) {
428         interrupt_restore_priority(level);
429         GI2MIC_LOG(LOG_ERR, "%s - Error (%d) to write register: phy_addr 0x%x, "
430                    "reg 0x%x\n", __FUNCTION__, res, phy_addr, reg);
431 
432         return res;
433     }
434 
435     /*
436      * Restore interrupts
437      */
438     interrupt_restore_priority(level);
439 
440 #ifdef VSC8584_DEBUG
441     GI2MIC_LOG(LOG_DEBUG, "%s: write register: phy_addr 0x%x,reg 0x%x, data 0x%x\n",
442                __FUNCTION__, phy_addr, reg, *data);
443 #endif
444 
445     return EOK;
446 }
447 
448 status_t
gi2mic_cl37_reg_read(vsc8584_phy_t * obj,u_int16_t reg_addr,u_int16_t * data)449 gi2mic_cl37_reg_read (vsc8584_phy_t *obj, u_int16_t reg_addr, u_int16_t *data)
450 {
451     int rv;
452 
453     CHECK_NULLOBJ;
454 
455     GI2MIC_LOCK(((vsc8584_phy_t *)obj)->mic_slot);
456     /* enable 10G  mdio access enable */
457     GI2MIC_CPLD_GE_MODE_ENABLE(((vsc8584_phy_t *)obj), GE_MODE_DIS);
458 
459     rv = gi2mic_cl37_mdio_reg_read (obj, reg_addr, data);
460 
461     GI2MIC_UNLOCK(((vsc8584_phy_t *)obj)->mic_slot);
462 
463     return rv;
464 }
465 
466 status_t
gi2mic_cl37_reg_write(vsc8584_phy_t * obj,u_int16_t reg_addr,u_int16_t * data)467 gi2mic_cl37_reg_write (vsc8584_phy_t *obj, u_int16_t reg_addr, u_int16_t *data)
468 {
469     int     rv;
470 
471     CHECK_NULLOBJ;
472 
473     GI2MIC_LOCK(((vsc8584_phy_t *)obj)->mic_slot);
474     /* enable 10G  mdio access enable */
475     GI2MIC_CPLD_GE_MODE_ENABLE(((vsc8584_phy_t *)obj), GE_MODE_DIS);
476 
477     rv = gi2mic_cl37_mdio_reg_write (obj, reg_addr, data);
478 
479     GI2MIC_UNLOCK(((vsc8584_phy_t *)obj)->mic_slot);
480 
481     return rv;
482 }
483 
484 
485 static status_t
gi2mic_miim_reg_read(vtss_inst_t inst,int port_no,u_int16_t reg,u_int16_t * data)486 gi2mic_miim_reg_read (vtss_inst_t inst, int port_no,
487                       u_int16_t  reg, u_int16_t *data)
488 {
489    void *obj = gi2mic_get_vsc8584_obj(inst, port_no);
490 
491    CHECK_NULLOBJ;
492 
493    return gi2mic_cl37_mdio_reg_read (obj, reg, data);
494 }
495 
496 static status_t
gi2mic_miim_reg_write(vtss_inst_t inst,int port_no,u_int16_t reg,u_int16_t data)497 gi2mic_miim_reg_write (vtss_inst_t inst, int port_no,
498                        u_int16_t  reg, u_int16_t data)
499 {
500   void *obj = gi2mic_get_vsc8584_obj(inst, port_no);
501 
502   CHECK_NULLOBJ;
503 
504   return gi2mic_cl37_mdio_reg_write (obj, reg, &data);
505 }
506 
507 status_t
gi2mic_vsc8584_instance_init(vtss_inst_t inst,boolean warm_start)508 gi2mic_vsc8584_instance_init(vtss_inst_t inst, boolean warm_start)
509 {
510     vtss_init_conf_t    init_conf;
511 
512    /*
513     * Now configure vtss instance
514     */
515    vtss_init_conf_get((const vtss_inst_t)inst, &init_conf);
516 
517    /*
518     * Setup all platform specific callbacks for vtss APIs.
519     */
520    init_conf.miim_read  = (void *)gi2mic_miim_reg_read;
521    init_conf.miim_write = (void *)gi2mic_miim_reg_write;
522    init_conf.restart_info_src     = VTSS_RESTART_INFO_SRC_CU_PHY;
523    init_conf.restart_info_port     = 0;
524 
525    init_conf.warm_start_enable    = warm_start;
526 
527    if (init_conf.warm_start_enable) {
528         vtss_restart_conf_set(inst, VTSS_RESTART_WARM);
529    } else {
530         vtss_restart_conf_set(inst, VTSS_RESTART_COLD);
531    }
532 
533    if (vtss_init_conf_set(inst, &init_conf) == VTSS_RC_ERROR) {
534        GI2MIC_LOG(LOG_ERR, "%s: vtss_init_conf_set failed\n", __FUNCTION__);
535        return EFAIL;
536    }
537 
538    if (vtss_restart_conf_end(inst) == VTSS_RC_ERROR) {
539         GI2MIC_LOG(LOG_ERR, "%s:vtss_restart_conf_end failed\n", __FUNCTION__);
540         return EFAIL;
541    }
542 
543    return EOK;
544 }
545 
546 static status_t
gi2mic_vsc8584_phy_config(vsc8584_phy_t * obj)547 gi2mic_vsc8584_phy_config(vsc8584_phy_t *obj)
548 {
549     status_t rc = EOK;
550     vtss_phy_conf_t phy;
551     vtss_phy_conf_1g_t phy_1g;
552     vtss_phy_reset_conf_t phy_resetCfg;
553 
554     bzero(&phy,sizeof(phy));
555     bzero(&phy_1g,sizeof(phy_1g));
556     bzero(&phy_resetCfg,sizeof(phy_resetCfg));
557 
558     /* Get the PHY config for port in the PHY Inst */
559     rc = vtss_phy_conf_get((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy);
560     if (rc != EOK) {
561         syslog(LOG_ERR, "%s: inst:%p: port:%d : config get failed\n",
562                __FUNCTION__, obj->inst,VSC8584_PORT);
563         return rc;
564     }
565 
566     /* Get the PHY 1G Conf for the Port from Inst */
567     rc = vtss_phy_conf_1g_get((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_1g);
568     if (rc != EOK) {
569         syslog(LOG_ERR, "%s: inst:%p: port:%d : 1G config get failed\n",
570                __FUNCTION__, obj->inst,VSC8584_PORT);
571         return rc;
572     }
573 
574     /* Get the PHY 1G Reset Cfg for the Port */
575     rc = vtss_phy_reset_get((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_resetCfg);
576     if (rc != EOK) {
577         syslog(LOG_ERR, "%s: inst:%p: port:%d : reset config get failed\n",
578                __FUNCTION__, obj->inst,VSC8584_PORT);
579         return rc;
580     }
581 
582     phy.mode = VTSS_PHY_MODE_ANEG;
583 
584     /* Example for PHY speed support for auto-neg */
585     phy.aneg.speed_10m_hdx = 1;
586     phy.aneg.speed_10m_fdx = 1;
587     phy.aneg.speed_100m_hdx = 1;
588     phy.aneg.speed_100m_fdx = 1;
589     phy.aneg.speed_1g_hdx = 0;
590     phy.aneg.speed_1g_fdx = 1;
591 
592     // Example for PHY flow control settings
593     phy.aneg.symmetric_pause =  0;
594     phy.aneg.tx_remote_fault =  0;
595 
596     phy.forced.speed = VTSS_SPEED_1G;
597     phy.forced.fdx = 1;
598 
599     phy.mdi = VTSS_PHY_MDIX_AUTO; // always enable auto detection of crossed/non-crossed cables
600 
601     phy.flf = VTSS_PHY_FAST_LINK_FAIL_ENABLE;
602 
603     phy.sigdet = VTSS_PHY_SIGDET_POLARITY_ACT_LOW;   /* For SFP, this is normally connected to LOS */
604 
605     phy.unidir = VTSS_PHY_UNIDIRECTIONAL_DISABLE;
606 
607     /*  Setup the MAC Interface PCS Parameters */
608     phy.mac_if_pcs.disable = 1; /* Disable MAC i/f when media link down */
609     phy.mac_if_pcs.restart = 0;
610     phy.mac_if_pcs.pd_enable = 0;
611     phy.mac_if_pcs.aneg_restart = 0;
612     phy.mac_if_pcs.force_adv_ability = 0;
613     phy.mac_if_pcs.sgmii_in_pre = VTSS_PHY_MAC_SERD_PCS_SGMII_IN_PRE_NONE;
614     phy.mac_if_pcs.sgmii_out_pre = 1;
615     phy.mac_if_pcs.serdes_aneg_ena = 0;
616     phy.mac_if_pcs.serdes_pol_inv_in = 0;
617     phy.mac_if_pcs.serdes_pol_inv_out = 0;
618     phy.mac_if_pcs.fast_link_stat_ena = 0;
619     phy.mac_if_pcs.inhibit_odd_start = 1;
620 
621     /* Setup the MEDIA Interface PCS Parameters */
622     phy.media_if_pcs.remote_fault = VTSS_PHY_MEDIA_SERD_PCS_REM_FAULT_NO_ERROR;
623     phy.media_if_pcs.aneg_pd_detect = 1;
624     phy.media_if_pcs.force_adv_ability = 0;
625     phy.media_if_pcs.inhibit_odd_start = 1;
626     phy.media_if_pcs.force_hls = 0;
627     phy.media_if_pcs.force_fefi = 0;
628     phy.media_if_pcs.force_fefi_value = 0;
629 
630     /* Setup PHY 1G config for Master/Slave relationship */
631     phy_1g.master.cfg = 0;    /* Master/Slave config, 1=enabled, 0=disable */
632     phy_1g.master.val = 0;    /* Master/Slave config, 1=Master, 0=Slave */
633 
634     phy_resetCfg.mac_if = VTSS_PORT_INTERFACE_SGMII;             /* Set the port MAC interface */
635     phy_resetCfg.media_if = VTSS_PHY_MEDIA_IF_FI_1000BX;         /* Set media interface for 1000BaseX */
636     phy_resetCfg.tbi.aneg_enable = FALSE;  /* Set to Enable tbi */
637     phy_resetCfg.force = VTSS_PHY_FORCE_RESET;
638     phy_resetCfg.pkt_mode = VTSS_PHY_PKT_MODE_JUMBO_9_KB;
639     phy_resetCfg.i_cpu_en = 0;  /* Set to TRUE to enable internal 8051 CPU for Enzo and Spyder Family Only */
640 
641     /* Note: API 4.67.03 and Later Automatically Applies the PHY Conf and PHY Conf_1g after Reset */
642     /* Therefore, there is No issue with setting the Config up PRIOR to the PHY Reset                            */
643     /* See Description of Reg23 in Data Sheet, See the Note for setting Reg23.13:8                                */
644     if (!(PIC_IN_ISSU_WINDOW(obj->pic))) {
645     rc = vtss_phy_reset((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_resetCfg);
646     if (rc != EOK) {
647         syslog(LOG_ERR, "%s: inst:%p: port:%d : reset config set failed\n",
648                __FUNCTION__, obj->inst,VSC8584_PORT);
649         return rc;
650     }
651 
652     rc = vtss_phy_conf_set((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy);
653     if (rc != EOK) {
654         syslog(LOG_ERR, "%s: inst:%p: port:%d : config set failed\n",
655                __FUNCTION__, obj->inst,VSC8584_PORT);
656         return rc;
657     }
658 
659     rc = vtss_phy_conf_1g_set((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_1g);
660     if (rc != EOK) {
661         syslog(LOG_ERR, "%s: inst:%p: port:%d : 1G config set failed\n",
662                __FUNCTION__, obj->inst,VSC8584_PORT);
663         return rc;
664     }
665     }
666     return EOK;
667 }
668 
669 status_t
gi2mic_vsc8584_phy_init(vsc8584_phy_t * obj)670 gi2mic_vsc8584_phy_init(vsc8584_phy_t *obj)
671 {
672     status_t rc = EOK;
673     mic_t  *dev;
674     int    link;
675     pic_t          *pic = NULL;
676     CHECK_NULLOBJ;
677 
678     if (mic_link_from_ifd(obj->ifd, &dev, &link) != EOK) {
679         return EFAIL;
680     }
681 
682     pic = dev->pic_context;
683 
684     if (!pic) {
685         syslog(LOG_ERR, "%s: Invalid PIC context\n", __FUNCTION__);
686         return EFAIL;
687     }
688 
689     if((PIC_IN_ISSU_WINDOW(obj->pic))) {
690         syslog(LOG_DEBUG, "%s: PIC_IN_ISSU_WINDOW (state=%d), returning\n",
691                           __FUNCTION__, ukern_state_get());
692         return EOK;
693     } else {
694 #ifdef VSC8584_DEBUG
695         syslog(LOG_DEBUG, "%s: PIC not in ISSU window (state=%d)\n",
696                           __FUNCTION__, ukern_state_get());
697 #endif
698     }
699     /* release PHY from reset */
700     if (gi2mic_vsc8584_1g_port_map[VSC8584_PORT].pre_reset_enable) {
701         GI2MIC_CPLD_RESET_PHY_ENABLE((GI2MIC_MEZZ_I2CS_VSC8584_PHY_0_RESET << obj->phy_idx), TRUE);
702 
703         rc = vtss_phy_pre_reset((const vtss_inst_t)obj->inst, VSC8584_PORT);
704         if (rc != VTSS_RC_OK) {
705             GI2MIC_LOG(LOG_ERR, "%s: pre-reset ==> %s, %s: for port: 0x%x, "
706                        "logical_port: %d\n", __FUNCTION__,
707                        (rc == VTSS_RC_OK) ? "success" : "failed",
708                        obj->ifd->ifd_name, VSC8584_PORT, obj->port_no);
709             if (mic_link_from_ifd(obj->ifd, &dev, &link) == EOK) {
710                 RESILIENCY_ERR_LOG(dev, MIC_ERROR_PHY_INIT_FAIL,
711                          "%s: %s: phy pre-reset failed\n",
712                          __FUNCTION__, obj->ifd->ifd_name);
713             }
714             return rc;
715         }
716     }
717     /* initialise the phy with a common ifd */
718     gi2mic_vsc8584_phy_config(obj);
719     if (EOK != rc) {
720         return rc;
721     }
722 
723     /* Up to this point, COMA mode should be Active, therefore the Ext. Interfaces of the PHY are down */
724     /* Post Reset code releases the Coma mode, which activates the External Interfaces */
725     /* Therefore, Depending upon your board layout, If you are using COMA mode to bring ALL ports up at once */
726     /* You may want to wait to execute vtss_phy_post_reset until ALL ports have completed the vtss_phy_reset() */
727     /*  portion of the API sequence.   */
728     /* If the COMA mode pins are not tied together to act as ONE, then you can release COMA mode for each PHY individually */
729         if (gi2mic_vsc8584_1g_port_map[obj->phy_addr].post_reset_enable == 1) {
730            rc = vtss_phy_post_reset((const vtss_inst_t)obj->inst, VSC8584_PORT);
731            if (rc != EOK) {
732                GI2MIC_LOG(LOG_DEBUG, "%s: post-reset ==> %s, for port: 0x%x, logical_port: %d\n",
733                            __FUNCTION__, (rc == EOK) ? "success" : "failed", VSC8584_PORT,
734                            obj->port_no);
735 
736                if (mic_link_from_ifd(obj->ifd, &dev, &link) == EOK) {
737                    RESILIENCY_ERR_LOG(dev, MIC_ERROR_PHY_INIT_FAIL,
738                          "%s: %s: phy port init failed\n",
739                          __FUNCTION__, obj->ifd->ifd_name);
740                }
741 
742                return rc;
743            }
744         }
745     return rc;
746 }
747 
748 /*
749  * gi2mic_vsc8584_instance_create
750  * This function creates a vsc8584 object per ifd
751  * Inputs - ifd
752  *        - phy_addr to use
753  *        - mdio_func
754  */
755 vtss_inst_t
gi2mic_vsc8584_instance_create(vtss_inst_t inst)756 gi2mic_vsc8584_instance_create(vtss_inst_t inst)
757 {
758     vtss_inst_create_t  vcreate;
759     vtss_target_type_t   target = VTSS_TARGET_CU_PHY;
760 
761     if (!inst) {
762         /*
763          * One vtss instance per phy chip
764          */
765         target = VTSS_TARGET_CU_PHY;
766         vtss_inst_get(target, &vcreate);
767         if (vtss_inst_create(&vcreate, &inst) != VTSS_RC_OK) {
768             syslog(LOG_ERR, "%s: vtss_inst_create failed\n", __FUNCTION__);
769             return NULL;
770         }
771    }
772    return inst;
773 }
774 
775 status_t
gi2mic_vsc8584_instance_destroy(vtss_inst_t inst)776 gi2mic_vsc8584_instance_destroy(vtss_inst_t inst)
777 {
778     status_t rc = EFAIL;
779 
780     if (inst) {
781         int i = 0;
782 
783         for (i = 0; i < MAX_MICS_PER_FPC; i++) {
784             if (vsc8584_handle.inst[i] == inst) {
785                 syslog(LOG_DEBUG,"%s: destroying instance\n",__FUNCTION__);
786 
787                 rc = vtss_inst_destroy(inst);
788                 if (rc != VTSS_RC_OK) {
789                     syslog(LOG_ERR, "%s: vtss_inst_destroy failed\n", __FUNCTION__);
790                 }
791 
792                 vsc8584_handle.inst[i] = 0;
793             }
794         }
795    }
796 
797    return rc;
798 }
799 
800 static status_t
gi2mic_vsc8584_obj_init(int fpc_slot,int pic_slot,ifd_t * ifd,lnk_mdio_vector_t * mdio_func)801 gi2mic_vsc8584_obj_init (int fpc_slot, int pic_slot, ifd_t *ifd,
802                          lnk_mdio_vector_t *mdio_func)
803 {
804     vsc8584_phy_t *obj;
805     int inst_idx = 0;
806     int vsc_port = 0;
807     int status = 0;
808     int do_instance_init = 0;
809     boolean warm_start = FALSE;
810 
811 
812     inst_idx = GI2MIC_VSC8584_INST_INDEX(fpc_slot, pic_slot);
813 
814     if (vsc8584_ctrl.num_dev[inst_idx])
815         return EOK;
816 
817     for (vsc_port = 0; vsc_port < MAX_VSC8584_PORTS_PER_MIC; vsc_port++) {
818         obj = (vsc8584_phy_t *)kzalloc(sizeof(vsc8584_phy_t));
819         if (!obj) {
820             syslog(LOG_ERR, "%s: can not allocate memory\n", __FUNCTION__);
821             goto init_fail;
822         }
823 
824         obj->phy_addr  = vsc_port;
825         obj->fpc_slot  = fpc_slot;
826         obj->pic_slot  = pic_slot;
827         obj->mic_slot  = pic_slot / MAX_PICS_PER_MIC;
828         obj->phy_idx = vsc_port / 4;
829         obj->use_spi = 0;
830         obj->port_no = vsc_port % 4;
831         obj->link_state_ok = FALSE;
832         obj->magic     = VSC8584_MAGIC;
833 
834         obj->ifd       = ifd;
835         obj->mdio_func = mdio_func;
836 
837         VSC8584_GET_PIC_FROM_IFD(ifd, obj->pic, status);
838 
839 #ifdef VSC8584_DEBUG
840         syslog(LOG_DEBUG, "%s: create PHY obj, index:%d, ifd_name: %s, phy_addr : 0x%d, "
841                           "phy_idx: 0x%x, port_no: 0x%x\n",
842                           __FUNCTION__, obj->itable_index, ifd->ifd_name,
843                           obj->phy_addr, obj->phy_idx, obj->port_no);
844 #endif
845 
846         /* save it to itable */
847         itable16_alloc(vsc8584_ctrl.itable, obj, &obj->itable_index);
848 
849         vsc8584_ctrl.num_dev[GI2MIC_VSC8584_INST_INDEX(fpc_slot, pic_slot)]++;
850 
851 #ifdef VSC8585_DEBUG
852         vsc8584_print_vsc(obj->fpc_slot, obj->pic_slot, obj->link);
853         syslog(LOG_DEBUG,"%s: fpc_slot = %d, pic_slot = %d, link = %d __Done\n",
854                          __FUNCTION__, obj->fpc_slot,obj->pic_slot,obj->link);
855 #endif
856 
857         if (!vsc8584_handle.inst[inst_idx]) {
858             vsc8584_handle.inst[inst_idx] =
859                 vsc8584_instance_create(vsc8584_handle.inst[inst_idx]);
860             do_instance_init = 1;
861         }
862 
863         if (!vsc8584_handle.inst[inst_idx]) {
864             goto init_fail;
865         }
866 
867         obj->inst = vsc8584_handle.inst[inst_idx];
868         /* Check if PIC is in ISSU */
869         if (PIC_IN_ISSU_WINDOW(obj->pic)) {
870             warm_start =  TRUE;
871         } else {
872             warm_start = FALSE;
873         }
874         if (do_instance_init) {
875             gi2mic_vsc8584_instance_init(obj->inst,warm_start);
876             do_instance_init = 0;
877         }
878         gi2mic_vsc8584_phy_init(obj);
879 
880         thread_yield();
881     }
882 
883     return EOK;
884 
885 init_fail:
886     for (vsc_port = 0; vsc_port < MAX_VSC8584_PORTS_PER_MIC; vsc_port++) {
887         obj = itable16_lookup(vsc8584_ctrl.itable, (index16_t *)&vsc_port);
888         if (obj) {
889             itable16_remove(vsc8584_ctrl.itable, (index16_t *)&vsc_port);
890             free(obj);
891         }
892     }
893 
894     vsc8584_instance_destroy(vsc8584_handle.inst[inst_idx]);
895     vsc8584_handle.inst[inst_idx] = NULL;
896 
897     return EFAIL;
898 }
899 
900 vsc8584_phy_t *
gi2mic_vsc8584_init(ifd_t * ifd,u_int16_t phy_addr,lnk_mdio_vector_t * mdio_func)901 gi2mic_vsc8584_init (ifd_t *ifd, u_int16_t phy_addr,
902                      lnk_mdio_vector_t *mdio_func)
903 {
904     int             inst_idx = 0;
905     pic_t          *pic = NULL;
906     int             status = 0;
907     vsc8584_phy_t    *obj = NULL;
908 
909     VSC8584_GET_PIC_FROM_IFD(ifd, pic, status);
910 
911     if (!pic) {
912         syslog(LOG_ERR, "%s: Invalid PIC context\n", __FUNCTION__);
913         return NULL;
914     }
915 
916     if (EOK != gi2mic_vsc8584_obj_init(pic->nic_slot, pic->pic_slot,
917                                        ifd, mdio_func)) {
918         return NULL;
919     }
920 
921     /* associate obj with respective ifd */
922     inst_idx = GI2MIC_VSC8584_INST_INDEX(pic->nic_slot, pic->pic_slot);
923 
924     obj = gi2mic_get_vsc8584_obj(vsc8584_handle.inst[inst_idx], phy_addr);
925     obj->mdio_func = mdio_func;
926     obj->ifd       = ifd;
927     obj->secy_threshold = mic_get_phy_port_secy_threshold(pic->pic_context);
928     obj->link      = ifd->ifd_subport;
929     strncpy(obj->dev_name,ifd->ifd_name,sizeof(obj->dev_name));
930 
931     return obj;
932 }
933 
934 status_t
gi2mic_vsc8584_port_init(vsc8584_phy_t * obj)935 gi2mic_vsc8584_port_init(vsc8584_phy_t *obj)
936 {
937     status_t rc = EOK;
938     u8 egr_fail = FALSE;
939     u8 ing_fail = FALSE;
940     mic_t  *dev;
941     int    link;
942     int gmic_slot = 0;
943 
944     CHECK_NULLOBJ;
945 
946     /* re-initialise the phy with the ifd*/
947     rc = gi2mic_vsc8584_phy_config(obj);
948     if (EOK != rc) {
949         return rc;
950     }
951     if (mic_link_from_ifd(obj->ifd, &dev, &link) != EOK) {
952         return EFAIL;
953     }
954     /* Save mic_type in obj structure*/
955     obj->mic_type = dev->mic_type;
956     /* Updating the vsc8584_stats_periodic_count*/
957     vsc8584_stats_periodic_count = GI2MIC_VSC_MACSEC_STATS_PERIODIC_INTERVAL;
958 
959     if (parser_get_fips_flag()  == TRUE) {
960         /*
961          * Setup vtss-api debug options
962          */
963         bzero(&vtss_appl_trace_conf,sizeof(vtss_appl_trace_conf));
964         vtss_appl_trace_conf.level[VTSS_TRACE_LAYER_AIL] = VTSS_TRACE_LEVEL_INFO;
965         vtss_appl_trace_conf.level[VTSS_TRACE_LAYER_CIL] = VTSS_TRACE_LEVEL_INFO;
966         vtss_trace_conf_set(VTSS_TRACE_GROUP_PHY, &vtss_appl_trace_conf);
967         vtss_trace_conf_set(VTSS_TRACE_GROUP_MACSEC, &vtss_appl_trace_conf);
968 
969         gmic_slot = obj->pic_slot;
970         if (mic_link_from_ifd(obj->ifd, &dev, &link) != EOK) {
971             GI2MIC_LOG(LOG_ERR, "%s: ifd %u not found\n",
972                        __FUNCTION__, obj->ifd->ifd_index);
973             return EFAIL;
974         }
975         if (services_get_fips_error(gmic_slot)) {
976             services_clear_fips_error(gmic_slot);
977             egr_fail = TRUE;
978             ing_fail = TRUE;
979             rc = EFAIL;
980         } else {
981             rc = vtss_phy_macsec_kat_test((const vtss_inst_t)obj->inst, VSC8584_PORT, &egr_fail, &ing_fail);
982         }
983         if (rc != EOK) {
984             services_set_fips_error(gmic_slot^1, 1);
985             GI2MIC_LOG(LOG_ERR, "%s: %s FIPS KATS port %d failed!\n",
986                        __FUNCTION__, ((vsc8584_phy_t *)obj)->dev_name, VSC8584_PORT);
987             syslog(LOG_INFO, "Return Code for %s KAT Test: %d  Egress Results: %x, Ingress_Results: %x\n",
988                    obj->ifd->ifd_name, rc, egr_fail, ing_fail);
989 
990             pic_set_state(obj->fpc_slot, obj->pic_slot , PIC_STATE_OFFLINE_REQ);
991             pic_set_offline_reason(obj->fpc_slot, obj->pic_slot, REASON_SPU_FIPS_ERROR);
992         } else {
993             GI2MIC_LOG(LOG_INFO, "%s: %s MIC%u/PIC%u/PHY%d port %d FIPS AES-256-GCM KATS passed\n", __FUNCTION__,
994                        obj->dev_name, obj->mic_slot,
995                        obj->pic_slot, obj->phy_idx, obj->port_no);
996         }
997     }
998 
999     return rc;
1000 }
1001 
1002 status_t
gi2mic_vsc8584_dev_reset(vsc8584_phy_t * obj)1003 gi2mic_vsc8584_dev_reset (vsc8584_phy_t *obj)
1004 {
1005     CHECK_NULLOBJ;
1006 
1007     if (gi2mic_vsc8584_1g_port_map[obj->phy_addr].pre_reset_enable == 1) {
1008         vtss_phy_pre_reset((const vtss_inst_t)obj->inst, VSC8584_PORT);
1009     }
1010 
1011     return vtss_phy_reset((const vtss_inst_t)obj->inst, VSC8584_PORT, &vsc8584_reset_conf);
1012 }
1013 
1014 status_t
gi2mic_vsc8584_enable_set(vsc8584_phy_t * obj,boolean enable)1015 gi2mic_vsc8584_enable_set(vsc8584_phy_t *obj, boolean enable)
1016 {
1017     vtss_phy_conf_t conf;
1018 
1019     CHECK_NULLOBJ;
1020 
1021     bzero(&conf,sizeof(conf));
1022     vtss_phy_conf_get((const vtss_inst_t)obj->inst, VSC8584_PORT, &conf);
1023     conf.mode = enable ? VTSS_PHY_MODE_FORCED : VTSS_PHY_MODE_POWER_DOWN;
1024     return vtss_phy_conf_set((const vtss_inst_t)obj->inst, VSC8584_PORT, &conf);
1025 }
1026 
1027 status_t
gi2mic_vsc8584_local_loopback_set(vsc8584_phy_t * obj,boolean onoff)1028 gi2mic_vsc8584_local_loopback_set(vsc8584_phy_t *obj, boolean onoff)
1029 {
1030     vtss_phy_loopback_t phy_lb_conf;
1031 
1032     CHECK_NULLOBJ;
1033 
1034     bzero(&phy_lb_conf,sizeof(phy_lb_conf));
1035 
1036     phy_lb_conf.mac_serdes_facility_enable = onoff;
1037 
1038     return vtss_phy_loopback_set((const vtss_inst_t)obj->inst, VSC8584_PORT, phy_lb_conf);
1039 }
1040 
1041 status_t
gi2mic_vsc8584_remote_loopback_set(vsc8584_phy_t * obj,boolean onoff)1042 gi2mic_vsc8584_remote_loopback_set(vsc8584_phy_t *obj, boolean onoff)
1043 {
1044     int status = 0;
1045     vtss_phy_loopback_t phy_lb_conf;
1046 
1047     CHECK_NULLOBJ;
1048 
1049     bzero(&phy_lb_conf,sizeof(phy_lb_conf));
1050 
1051     phy_lb_conf.media_serdes_facility_enable = onoff;
1052 
1053     status = vtss_phy_loopback_set((const vtss_inst_t)obj->inst, VSC8584_PORT, phy_lb_conf);
1054 
1055     return status;
1056 
1057 }
1058 
1059 status_t
gi2mic_vsc8584_link_status_get(vsc8584_phy_t * obj,boolean * link)1060 gi2mic_vsc8584_link_status_get(vsc8584_phy_t *obj ,boolean *link)
1061 {
1062     status_t rv = EFAIL;
1063     vtss_port_status_t port_status;
1064     ifd_t     *ifd;
1065     u_int16_t data_pma          = 0x0;
1066     status_t  status            = EFAIL;
1067     boolean   link_ok           = FALSE;
1068     vtss_phy_reset_conf_t phy_resetCfg;
1069     mic_t     *dev;
1070     int       phy_link;
1071 
1072     CHECK_NULLOBJ;
1073 
1074     ifd       = obj->ifd;
1075 
1076 
1077     bzero(&port_status, sizeof(port_status));
1078     bzero(&phy_resetCfg, sizeof(phy_resetCfg));
1079 
1080    rv = vtss_phy_reset_get(obj->inst, VSC8584_PORT, &phy_resetCfg);
1081 
1082    rv = vtss_phy_status_get(obj->inst, VSC8584_PORT, &port_status);
1083 
1084     *link = port_status.link;
1085 
1086     /* Now read the PMA/PMD register */
1087     status = vsc8584_reg_read (obj, 0, 0, VSC8584_PMA_PMD_REG, &data_pma);
1088     if (status != EOK) {
1089         syslog(LOG_ERR, "%s: Failed reading vsc8584 PMA/PMD reg for ifd %s\n",
1090                 __FUNCTION__, ifd->ifd_name);
1091         return EFAIL;
1092     }
1093 
1094     /* Check the 2nd bit of PMA/PMD register */
1095     link_ok = ((data_pma  & VSC8584_PMA_PMD_REGISTER_LINK_STATUS) != 0) ?
1096                TRUE:FALSE;
1097     if (!link_ok) {
1098         /*
1099          * Low latching - Read it again
1100          */
1101         status = vsc8584_reg_read (obj, 0, 0, VSC8584_PMA_PMD_REG, &data_pma);
1102         if (status != EOK) {
1103             syslog(LOG_ERR, "%s: Failed reading vsc8584 PMA/PMD reg for ifd %s\n",
1104                     __FUNCTION__, ifd->ifd_name);
1105             return EFAIL;
1106         }
1107 
1108         link_ok = ((data_pma  & VSC8584_PMA_PMD_REGISTER_LINK_STATUS) != 0) ?
1109                    TRUE:FALSE;
1110 
1111         if (!link_ok) {
1112 #ifdef VSC8584_DEBUG
1113             syslog(LOG_DEBUG, "%s: IFD %s - vsc8584 PHY PMA/PMD reg = %x,"
1114                     " link_ok = %d, obj->link_state_ok = %d\n",
1115                     __FUNCTION__, ifd->ifd_name, data_pma, link_ok,
1116                     obj->link_state_ok);
1117 #endif
1118             /*
1119              * Workaround: The link remains UP even after the fiber is plugged out
1120              * in case of SFP-FX-PHY. vtss_phy_status_get checks the link partner
1121              * ability and returns link as up for some reason. While this needs
1122              * to be taken up with vendor, implementing a workaround
1123              * to set the status based on the PMA/PMD register alone.
1124              */
1125             if (mic_link_from_ifd(ifd, &dev, &phy_link) != EOK) {
1126                 syslog (LOG_ERR, "%s: Failed to fetch MIC from the ifd %s\n",
1127                                  __FUNCTION__, ifd->ifd_name);
1128                 return EFAIL;
1129             }
1130 
1131             if (dev->sfp_optical[phy_link].sfp &&
1132                 dev->sfp_optical[phy_link].sfp->port_cable_type == PIC_PORT_GIGE_100_FX_PHY) {
1133                 *link = link_ok;
1134             }
1135         }
1136         /* Link has gone down as compared to last updated state in phy obj */
1137         if((obj->link_state_ok == TRUE) && (obj->link_state_ok !=  link_ok)) {
1138             syslog(LOG_NOTICE, "%s: PHY LINK down detected for ifd %s\n",
1139                                 __FUNCTION__, ifd->ifd_name);
1140             syslog(LOG_INFO, "\tvsc8584 PMA/PMD reg for this ifd reads %x\n",
1141                               data_pma);
1142         }
1143     }
1144 
1145     /* Update the phy object member with latest link_ok status from PMA */
1146     obj->link_state_ok = link_ok;
1147     if (phy_resetCfg.media_if == VTSS_PHY_MEDIA_IF_SFP_PASSTHRU) {
1148         if (ifd->ifd_flags & IFF_DOWN) {
1149             *link = FALSE;
1150         }
1151     }
1152 
1153     return rv;
1154 }
1155 
1156 status_t
gi2mic_vsc8584_speed_get(vsc8584_phy_t * obj,int * speed)1157 gi2mic_vsc8584_speed_get(vsc8584_phy_t *obj, int *speed)
1158 {
1159     int    rv = EFAIL;
1160     vtss_port_status_t port_status;
1161 
1162     CHECK_NULLOBJ;
1163 
1164     bzero(&port_status,sizeof(port_status));
1165 
1166     rv = vtss_phy_status_get((const vtss_inst_t)obj->inst, VSC8584_PORT, &port_status);
1167 
1168     if (port_status.link == TRUE) {
1169         switch (port_status.speed) {
1170         case VTSS_SPEED_10M:
1171              *speed = LNK_10_MEGABIT;
1172              break;
1173         case VTSS_SPEED_100M:
1174              *speed = LNK_100_MEGABIT;
1175              break;
1176         case  VTSS_SPEED_1G:
1177              *speed = LNK_1_GIGABIT;
1178              break;
1179         default:
1180              *speed = port_status.speed;
1181              return EFAIL;
1182         }
1183     }
1184 
1185     return rv;
1186 }
1187 
1188 status_t
gi2mic_vsc8584_speed_set(vsc8584_phy_t * obj,int speed)1189 gi2mic_vsc8584_speed_set (vsc8584_phy_t *obj, int speed)
1190 {
1191     vtss_phy_conf_t phy_conf;
1192 
1193     CHECK_NULLOBJ;
1194 
1195     bzero(&phy_conf,sizeof(phy_conf));
1196 
1197     vtss_phy_conf_get((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_conf);
1198 
1199     switch (speed) {
1200         case LNK_10_MEGABIT:
1201             phy_conf.forced.speed = VTSS_SPEED_10M;
1202             break;
1203         case LNK_100_MEGABIT:
1204             phy_conf.forced.speed = VTSS_SPEED_100M;
1205             break;
1206         case LNK_1_GIGABIT:
1207             phy_conf.forced.speed = VTSS_SPEED_1G;
1208             break;
1209         default:
1210             return EFAIL;
1211     }
1212 
1213     return vtss_phy_conf_set((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_conf);
1214 }
1215 
1216 status_t
gi2mic_vsc8584_duplex_set(vsc8584_phy_t * obj,int duplex)1217 gi2mic_vsc8584_duplex_set(vsc8584_phy_t *obj, int duplex)
1218 {
1219     vtss_phy_conf_t phy_conf;
1220 
1221     CHECK_NULLOBJ;
1222 
1223     bzero(&phy_conf,sizeof(phy_conf));
1224 
1225     vtss_phy_conf_get((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_conf);
1226 
1227     phy_conf.forced.fdx = duplex;
1228 
1229     return vtss_phy_conf_set((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_conf);
1230 }
1231 
1232 status_t
gi2mic_vsc8584_duplex_get(vsc8584_phy_t * obj,boolean * duplex)1233 gi2mic_vsc8584_duplex_get(vsc8584_phy_t *obj, boolean *duplex)
1234 {
1235     int    rv = EFAIL;
1236     vtss_port_status_t port_status;
1237 
1238     CHECK_NULLOBJ;
1239 
1240     bzero(&port_status,sizeof(port_status));
1241 
1242     rv = vtss_phy_status_get((const vtss_inst_t)obj->inst, VSC8584_PORT, &port_status);
1243 
1244     if (port_status.link == TRUE) {
1245         *duplex = port_status.fdx;
1246     }
1247 
1248     return rv;
1249 }
1250 
1251 status_t
gi2mic_vsc8584_update_lpinfo(vsc8584_phy_t * obj,void * ge_status)1252 gi2mic_vsc8584_update_lpinfo (vsc8584_phy_t *obj, void *ge_status)
1253 {
1254     CHECK_NULLOBJ;
1255 
1256     return vtss_phy_eee_link_partner_advertisements_get((const vtss_inst_t)obj->inst, VSC8584_PORT, ge_status);
1257 }
1258 
1259 status_t
gi2mic_vsc8584_an_init(vsc8584_phy_t * obj,void * ptr_an_adv,boolean * change)1260 gi2mic_vsc8584_an_init(vsc8584_phy_t *obj , void *ptr_an_adv, boolean *change)
1261 {
1262     an_adv_t *an_adv;
1263     vtss_phy_conf_t phy_conf;
1264 
1265     CHECK_NULLOBJ;
1266 
1267     bzero(&phy_conf, sizeof(phy_conf));
1268 
1269     an_adv = (an_adv_t *)ptr_an_adv;
1270 
1271     vtss_phy_conf_get((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_conf);
1272 
1273     if (an_adv->speed & AN_SPEED_TEN_HD) {
1274         phy_conf.aneg.speed_10m_hdx = TRUE;
1275     }
1276 
1277     if (an_adv->speed & AN_SPEED_TEN_FD) {
1278         phy_conf.aneg.speed_10m_fdx = TRUE;
1279     }
1280 
1281     if (an_adv->speed & AN_SPEED_HUN_HD) {
1282         phy_conf.aneg.speed_100m_hdx = TRUE;
1283     }
1284 
1285     if (an_adv->speed & AN_SPEED_HUN_FD) {
1286         phy_conf.aneg.speed_100m_fdx = TRUE;
1287     }
1288 
1289     if (an_adv->speed & AN_SPEED_GIG_HD) {
1290         phy_conf.aneg.speed_1g_hdx = TRUE;
1291     }
1292 
1293     if (an_adv->speed & AN_SPEED_GIG_FD) {
1294         phy_conf.aneg.speed_1g_fdx = TRUE;
1295     }
1296 
1297     phy_conf.mode = VTSS_PHY_MODE_ANEG;
1298 
1299     return vtss_phy_conf_set((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_conf);
1300 }
1301 
1302 status_t
gi2mic_vsc8584_an_set(vsc8584_phy_t * obj,int an)1303 gi2mic_vsc8584_an_set(vsc8584_phy_t *obj,int an)
1304 {
1305     vtss_phy_conf_t phy_conf;
1306 
1307     CHECK_NULLOBJ;
1308 
1309     bzero(&phy_conf,sizeof(phy_conf));
1310 
1311     vtss_phy_conf_get((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_conf);
1312 
1313     phy_conf.mode = VTSS_PHY_MODE_ANEG;
1314 
1315     return vtss_phy_conf_set((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_conf);
1316 }
1317 
1318 status_t
gi2mic_vsc8584_an_restart(vsc8584_phy_t * obj)1319 gi2mic_vsc8584_an_restart(vsc8584_phy_t *obj)
1320 {
1321     status_t rv = EFAIL;
1322     vtss_phy_conf_t phy_conf;
1323 
1324     CHECK_NULLOBJ;
1325 
1326     bzero(&phy_conf,sizeof(phy_conf));
1327 
1328     vtss_phy_conf_get((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_conf);
1329 
1330     if (phy_conf.mode == VTSS_PHY_MODE_ANEG) {
1331         vsc8584_an_set(obj,FALSE);
1332         rv = vtss_phy_conf_set((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_conf);
1333     }
1334 
1335     return rv;
1336 }
1337 
1338 status_t
gi2mic_vsc8584_an_get(vsc8584_phy_t * obj,boolean * an)1339 gi2mic_vsc8584_an_get(vsc8584_phy_t *obj, boolean *an)
1340 {
1341     status_t rv = EOK;
1342     vtss_phy_conf_t phy_conf;
1343 
1344     CHECK_NULLOBJ;
1345 
1346     bzero(&phy_conf,sizeof(phy_conf));
1347 
1348     rv = vtss_phy_conf_get((const vtss_inst_t)obj->inst, VSC8584_PORT, &phy_conf);
1349 
1350     if (phy_conf.mode == VTSS_PHY_MODE_ANEG) {
1351         *an = TRUE;
1352     } else {
1353         *an = FALSE;
1354     }
1355 
1356     return rv;
1357 }
1358 
1359 /*
1360  * gi2mic_vsc8584_get_link_intr_status:
1361  * Get the Link status change interrupt from the Exp register
1362  */
1363 status_t
gi2mic_vsc8584_get_link_intr_status(vsc8584_phy_t * obj,boolean * link_intr_status)1364 gi2mic_vsc8584_get_link_intr_status (vsc8584_phy_t *obj,
1365                                      boolean * link_intr_status)
1366 {
1367     status_t rv = EOK;
1368     vtss_phy_event_t ev_mask = 0;
1369 
1370     CHECK_NULLOBJ;
1371 
1372     rv = vtss_phy_event_poll((const vtss_inst_t)obj->inst, VSC8584_PORT, &ev_mask);
1373 
1374     *link_intr_status = ev_mask ? TRUE : FALSE;
1375 
1376     return  rv;
1377 }
1378 
1379 /*
1380  * vsc8584_link_intr_enable:
1381  * This function is used to enable/disable generation of
1382  * the link status change interrupt.
1383  */
1384 status_t
gi2mic_vsc8584_link_intr_enable(vsc8584_phy_t * obj,boolean on_off)1385 gi2mic_vsc8584_link_intr_enable (vsc8584_phy_t *obj,
1386                                  boolean  on_off)
1387 {
1388     vtss_phy_event_t ev_mask = ( VTSS_PHY_LINK_LOS_EV | VTSS_PHY_LINK_SPEED_STATE_CHANGE_EV);
1389 
1390     CHECK_NULLOBJ;
1391 
1392     return vtss_phy_event_enable_set((const vtss_inst_t)obj->inst, VSC8584_PORT, ev_mask, on_off);
1393 }
1394 #endif
1395 
1396 #ifndef VSC8490
1397 void *
gi2mic_get_vsc8490_obj(vtss_inst_t inst,u_int16_t port)1398 gi2mic_get_vsc8490_obj (vtss_inst_t inst, u_int16_t port)
1399 {
1400     vsc8490_phy_t *obj = NULL;
1401     index16_t temp_index = 1;
1402 
1403     /* index 1 is a starting index for vsc8490 */
1404     for (temp_index = 1; temp_index < vsc8490_ctrl.itable->itable16_maxentry; temp_index++) {
1405         obj = vsc8490_itable16_lookup(temp_index);
1406         if (obj) {
1407             if ((VSC8490_PORT == port)  &&
1408                  (obj->inst == inst)) {
1409                 return obj;
1410             }
1411         }
1412     }
1413 
1414     /* Not found after scanning all entries */
1415     return NULL;
1416 }
1417 
1418 int
gi2mic_get_vsc8490_port(vsc8490_phy_t * obj)1419 gi2mic_get_vsc8490_port(vsc8490_phy_t *obj)
1420 {
1421     CHECK_NULLOBJ;
1422 
1423     return ((obj->itable_index - 1)%(MAX_VSC8490_PORTS_PER_MIC));
1424 }
1425 
1426 /*
1427  * gi2mic_get_vsc8490_instance:
1428  *
1429  * typically the instances are managed 1 instance per mic
1430  * MX104 is an exception, where the 4 MICs are supported across 2 virtual FPs
1431  * in an AFEB. hence, there will be 4 instances supported across 2 FPCs catering to 2 MICs
1432  */
1433 int
gi2mic_get_vsc8490_instance(vsc8490_phy_t * obj __unused)1434 gi2mic_get_vsc8490_instance(vsc8490_phy_t *obj __unused)
1435 {
1436     return ((obj->fpc_slot % 2)*2 + (obj->mic_slot % 2));
1437 }
1438 
1439 int
gi2mic_get_vsc8490_itable_index(ifd_t * ifd __unused)1440 gi2mic_get_vsc8490_itable_index(ifd_t *ifd __unused)
1441 {
1442     return 0;
1443 }
1444 
1445 /*
1446  * gi2mic_cl45_mdio_reg_read
1447  * This function reads the vsc8490 phy using the registered
1448  * mdio access method
1449  */
1450 static status_t
gi2mic_cl45_mdio_reg_read(vsc8490_phy_t * obj,u_int8_t dev_addr,u_int16_t reg,u_int16_t * data)1451 gi2mic_cl45_mdio_reg_read (vsc8490_phy_t *obj, u_int8_t dev_addr, u_int16_t reg, u_int16_t *data)
1452 {
1453     status_t           res;
1454     lnk_mdio_vector_t *mdio_func;
1455     ifd_t             *ifd;
1456     u_int8_t           phy_addr;
1457     level_t            level;
1458 
1459     CHECK_NULLOBJ;
1460 
1461     mdio_func = obj->mdio_func;
1462     ifd       = obj->ifd;
1463     phy_addr  = obj->phy_addr;
1464 
1465     /*
1466      * BUG: with VSC SDK, the global page of a PHY is accessed through
1467      * logical port 0 of vsc8490. current SDK is accessing global page
1468      * with logical port index of the port in the PHY
1469      */
1470     if (dev_addr == GI2MIC_VSC8490_GLOBAL_DEV_ADDR) {
1471          phy_addr = GI2MIC_VSC8490_GLOBAL_PHY_ADDR(phy_addr);
1472     }
1473 
1474     /*
1475      * The MDIO is shared by all the ports/devices on the chip.
1476      * So the MDIO operation needs to be interrupt protected.
1477      * Raise interrupts. Interrupts are restored within 150usec.
1478      */
1479     level = interrupt_raise_priority(LEVEL_NETS);
1480 
1481     if ((res = mdio_func->mdio_xgmii_op( ifd,
1482                     phy_addr,
1483                     dev_addr,
1484                     reg,
1485                     LNK_MDIO_XGMII_READ,
1486                     LNK_MDIO_BUSY_WAIT_UNTIL_DONE,
1487                     data)) != EOK) {
1488         interrupt_restore_priority(level);
1489         GI2MIC_LOG(LOG_ERR, "%s - Error (%d) to read register: phy_addr 0x%x, "
1490                    "reg 0x%x\n", __FUNCTION__, res, phy_addr, reg);
1491 
1492         return res;
1493     }
1494     /*
1495      * Restore interrupts
1496      */
1497     interrupt_restore_priority(level);
1498 
1499 #ifdef VSC8490_DEBUG
1500     GI2MIC_LOG(LOG_DEBUG, "%s: read register: phy_addr 0x%x, "
1501                "reg 0x%x, data 0x%x\n",
1502                __FUNCTION__, phy_addr, reg, *data);
1503 #endif
1504 
1505     return EOK;
1506 }
1507 
1508 /*
1509  * gi2mic_l45_mdio_reg_write
1510  * This function writes to vsc8490 phy using the registered
1511  * mdio access method
1512  */
1513 static status_t
gi2mic_cl45_mdio_reg_write(vsc8490_phy_t * obj,u_int8_t dev_addr,u_int16_t reg,u_int16_t * data)1514 gi2mic_cl45_mdio_reg_write (vsc8490_phy_t *obj, u_int8_t dev_addr, u_int16_t reg, u_int16_t *data)
1515 {
1516     status_t            res;
1517     lnk_mdio_vector_t  *mdio_func;
1518     ifd_t              *ifd;
1519     u_int8_t            phy_addr;
1520     level_t             level;
1521 
1522     CHECK_NULLOBJ;
1523 
1524     mdio_func = obj->mdio_func;
1525     ifd       = obj->ifd;
1526     phy_addr  = obj->phy_addr;
1527 
1528     /*
1529      * BUG: with VSC SDK, the global page of a PHY is accessed through
1530      * logical port 0 of vsc8490. current SDK is accessing global page
1531      * with logical port index of the port in the PHY
1532      */
1533     if (dev_addr == GI2MIC_VSC8490_GLOBAL_DEV_ADDR) {
1534          phy_addr = GI2MIC_VSC8490_GLOBAL_PHY_ADDR(phy_addr);
1535     }
1536 
1537 #ifdef VSC8490_DEBUG
1538     GI2MIC_LOG(LOG_DEBUG,"%s: port = %d phy addr = 0x%x\n",__FUNCTION__,obj->link,phy_addr);
1539 #endif
1540 
1541     /*
1542      * The MDIO is shared by all the ports/devices on the chip.
1543      * So the MDIO operation needs to be interrupt protected.
1544      * Raise interrupts. Interrupts are restored within 150usec.
1545      */
1546     level = interrupt_raise_priority(LEVEL_NETS);
1547 
1548     if ((res = mdio_func->mdio_xgmii_op( ifd,
1549                     phy_addr,
1550                     dev_addr,
1551                     (u_int16_t)reg,
1552                     LNK_MDIO_XGMII_WRITE,
1553                     LNK_MDIO_BUSY_WAIT_UNTIL_DONE,
1554                     data)) != EOK) {
1555         interrupt_restore_priority(level);
1556         GI2MIC_LOG(LOG_ERR, "%s - Error (%d) to write register: phy_addr 0x%x, "
1557                    "reg 0x%x\n", __FUNCTION__, res, phy_addr, reg);
1558 
1559         return res;
1560     }
1561 
1562     /*
1563      * Restore interrupts
1564      */
1565     interrupt_restore_priority(level);
1566 
1567 #ifdef VSC8490_DEBUG
1568     GI2MIC_LOG(LOG_DEBUG, "%s: write register: phy_addr 0x%x, "
1569                "reg 0x%x, data 0x%x\n",
1570                __FUNCTION__, phy_addr, reg, *data);
1571 #endif
1572 
1573     return EOK;
1574 }
1575 
1576 /*
1577  * Callback functions needed by VTSS API
1578  */
1579 vtss_rc
gi2mic_vsc8490_mmd_read(const vtss_inst_t inst,const vtss_port_no_t port_no,const u_int8_t dev_id,const u_int16_t reg_addr,u_int16_t * const data)1580 gi2mic_vsc8490_mmd_read(const vtss_inst_t    inst,
1581                  const vtss_port_no_t port_no,
1582                  const u_int8_t       dev_id,
1583                  const u_int16_t      reg_addr,
1584                  u_int16_t            *const data)
1585 {
1586     status_t  ret;
1587     void *obj = gi2mic_get_vsc8490_obj(inst, port_no);
1588 
1589     CHECK_NULLOBJ;
1590 
1591     ret = gi2mic_cl45_mdio_reg_read(obj, dev_id, reg_addr, data);
1592 
1593     VSC8490_LOG_TRACE(VSC8490_DEBUG_FLAG_PHY_MDIO,
1594             "Read: port_no %d, dev_id %d, reg_addr 0x%x, data 0x%x\n",
1595             port_no, dev_id, reg_addr, *data);
1596     return (ret == EOK) ? VTSS_RC_OK : VTSS_RC_ERROR;
1597 }
1598 
1599 static vtss_rc
gi2mic_vsc8490_mmd_read_inc(const vtss_inst_t inst,const vtss_port_no_t port_no,const u_int8_t dev_id,const u_int16_t reg_addr,u_int16_t * const data,u_int8_t count)1600 gi2mic_vsc8490_mmd_read_inc(const vtss_inst_t    inst,
1601                      const vtss_port_no_t port_no,
1602                      const u_int8_t       dev_id,
1603                      const u_int16_t      reg_addr,
1604                      u_int16_t            *const data,
1605                      u_int8_t             count)
1606 {
1607     status_t  ret;
1608     int i = 0;
1609     vsc8490_phy_t *obj = gi2mic_get_vsc8490_obj(inst, port_no);
1610 
1611     CHECK_NULLOBJ;
1612 
1613     for (i = 0; i < count; i+=1) {
1614         ret = gi2mic_cl45_mdio_reg_read(obj, dev_id, (reg_addr + i), &data[i]);
1615     }
1616 
1617     VSC8490_LOG_TRACE(VSC8490_DEBUG_FLAG_PHY_MDIO, "Read: port_no %d, "
1618             "dev_id %d, reg_addr 0x%x, data 0x%x, count %d\n",
1619             port_no, dev_id, reg_addr, *data, count);
1620 
1621     return (ret == EOK) ? VTSS_RC_OK : VTSS_RC_ERROR;
1622 }
1623 
1624 static vtss_rc
gi2mic_vsc8490_mmd_write(const vtss_inst_t inst,const vtss_port_no_t port_no,const u_int8_t dev_id,const u_int16_t reg_addr,const u_int16_t data)1625 gi2mic_vsc8490_mmd_write(const vtss_inst_t    inst,
1626                   const vtss_port_no_t port_no,
1627                   const u_int8_t       dev_id,
1628                   const u_int16_t      reg_addr,
1629                   const u_int16_t      data)
1630 {
1631     status_t  ret;
1632     void *obj = gi2mic_get_vsc8490_obj(inst, port_no);
1633 
1634     CHECK_NULLOBJ;
1635 
1636     ret = gi2mic_cl45_mdio_reg_write (obj, dev_id, reg_addr, (u_int16_t *)&data);
1637 
1638     VSC8490_LOG_TRACE(VSC8490_DEBUG_FLAG_PHY_MDIO,
1639             "Write: port_no %d, dev_id %d, reg_addr 0x%x, data 0x%x\n",
1640             port_no, dev_id, reg_addr, data);
1641 
1642     return (ret == EOK) ? VTSS_RC_OK : VTSS_RC_ERROR;
1643 }
1644 
1645 status_t
gi2mic_cl45_reg_read(vsc8490_phy_t * obj,u_int8_t dev_addr,u_int16_t reg_addr,u_int16_t * data)1646 gi2mic_cl45_reg_read (vsc8490_phy_t *obj, u_int8_t dev_addr,
1647                       u_int16_t reg_addr, u_int16_t *data)
1648 {
1649     int rv;
1650 
1651     CHECK_NULLOBJ;
1652 
1653     GI2MIC_LOCK(((vsc8490_phy_t *)obj)->mic_slot);
1654     /* enable 10G  mdio access enable */
1655     GI2MIC_CPLD_GE_MODE_ENABLE(obj, GE_MODE_EN);
1656 
1657     rv = gi2mic_cl45_mdio_reg_read (obj, dev_addr, reg_addr, data);
1658 
1659     GI2MIC_UNLOCK(((vsc8490_phy_t *)obj)->mic_slot);
1660 
1661     return rv;
1662 }
1663 
1664 status_t
gi2mic_cl45_reg_read_inc(vsc8490_phy_t * obj,u_int8_t dev_addr,u_int16_t reg,u_int16_t * data,int count)1665 gi2mic_cl45_reg_read_inc (vsc8490_phy_t *obj, u_int8_t dev_addr,
1666                           u_int16_t reg, u_int16_t *data, int count)
1667 {
1668     CHECK_NULLOBJ;
1669 
1670     return gi2mic_vsc8490_mmd_read_inc(obj->inst, VSC8490_PORT,
1671                                        dev_addr, reg, data, count);
1672 }
1673 
1674 status_t
gi2mic_cl45_reg_write(vsc8490_phy_t * obj,u_int8_t dev_addr,u_int16_t reg_addr,u_int16_t * data)1675 gi2mic_cl45_reg_write (vsc8490_phy_t *obj, u_int8_t dev_addr,
1676                        u_int16_t reg_addr, u_int16_t *data)
1677 {
1678     int     rv;
1679 
1680     CHECK_NULLOBJ;
1681 
1682     GI2MIC_LOCK(((vsc8490_phy_t *)obj)->mic_slot);
1683     /* enable 10G  mdio access enable */
1684     GI2MIC_CPLD_GE_MODE_ENABLE(obj, GE_MODE_EN);
1685 
1686     rv = gi2mic_cl45_mdio_reg_write (obj, dev_addr, reg_addr, data);
1687 
1688     GI2MIC_UNLOCK(((vsc8490_phy_t *)obj)->mic_slot);
1689     return rv;
1690 }
1691 
1692 status_t
gi2mic_vsc8490_instance_init(vtss_inst_t inst,boolean warm_start)1693 gi2mic_vsc8490_instance_init(vtss_inst_t inst, boolean warm_start)
1694 {
1695     vtss_init_conf_t    init_conf;
1696 
1697     /*
1698      * Now configure vtss instance
1699      */
1700     vtss_init_conf_get(inst, &init_conf);
1701 
1702     /*
1703      * Setup all platform specific callbacks for vtss APIs.
1704      */
1705     init_conf.mmd_read = gi2mic_vsc8490_mmd_read;
1706     init_conf.mmd_read_inc = gi2mic_vsc8490_mmd_read_inc;
1707     init_conf.mmd_write = gi2mic_vsc8490_mmd_write;
1708     init_conf.spi_read_write = NULL;
1709     init_conf.spi_32bit_read_write = NULL;
1710     init_conf.restart_info_src     = VTSS_RESTART_INFO_SRC_10G_PHY;
1711     init_conf.restart_info_port     = 0;
1712     init_conf.warm_start_enable    = warm_start;
1713 
1714     /* Check this as hw configs are changing */
1715     if (init_conf.warm_start_enable) {
1716         syslog(LOG_DEBUG,"Sending warm boot signal from %s\n",__FUNCTION__);
1717         vtss_restart_conf_set(inst,
1718                               VTSS_RESTART_WARM);
1719     } else {
1720         vtss_restart_conf_set(inst,
1721                               VTSS_RESTART_COLD);
1722     }
1723       /* Set the phy configuration to the instance*/
1724     if (vtss_init_conf_set(inst, &init_conf) == VTSS_RC_ERROR) {
1725         GI2MIC_LOG(LOG_ERR, "%s: vtss_init_conf_set failed\n", __FUNCTION__);
1726         return EFAIL;
1727     }
1728     return EOK;
1729 }
1730 
1731 /*
1732  * gi2mic_vsc8490_port_init:
1733  *
1734  */
1735 status_t
gi2mic_vsc8490_port_init(vsc8490_phy_t * obj)1736 gi2mic_vsc8490_port_init(vsc8490_phy_t *obj)
1737 {
1738     status_t rc;
1739     vtss_phy_10g_mode_t   oper_mode;
1740     u_int8_t dev_addr = 0;
1741     u_int16_t reg_addr, val = 0;
1742     vtss_phy_10g_rxckout_conf_t rxckout_conf;
1743     mic_t  *dev;
1744     int    link;
1745     int gmic_slot = 0;
1746     pic_t  *pic = NULL;
1747     CHECK_NULLOBJ;
1748 
1749    /*
1750     *  Now configure vtss instance
1751     */
1752     if (mic_link_from_ifd(obj->ifd, &dev, &link) != EOK) {
1753         return EFAIL;
1754     }
1755     /* Save mic_type in obj structure*/
1756     obj->mic_type = dev->mic_type;
1757     /* Updating the vsc8490_stats_periodic_count */
1758     vsc8490_stats_periodic_count = GI2MIC_VSC_MACSEC_STATS_PERIODIC_INTERVAL;
1759 
1760     pic = dev->pic_context;
1761     if (!pic) {
1762         syslog(LOG_ERR, "%s: Invalid PIC context\n", __FUNCTION__);
1763         return EFAIL;
1764     }
1765 
1766     if (!(PIC_IN_ISSU_WINDOW(pic))) {
1767         /* release PHY from reset */
1768         GI2MIC_CPLD_RESET_PHY_ENABLE(GI2MIC_MEZZ_I2CS_VSC8490_PHY_0_RESET,
1769                                     TRUE);
1770     }
1771 
1772     memset(&oper_mode, 0 , sizeof(vtss_phy_10g_mode_t));
1773 
1774     /* VSC8490 port configuration */
1775     vtss_phy_10g_mode_get(obj->inst, VSC8490_PORT, &oper_mode);
1776 
1777     if (obj->ifd->ifd_port_type == IFDP_XSERIES_10G) {
1778         oper_mode.oper_mode = VTSS_PHY_LAN_MODE;
1779         oper_mode.interface = VTSS_PHY_XAUI_XFI;
1780     } else {
1781         oper_mode.oper_mode = VTSS_PHY_1G_MODE;
1782         oper_mode.interface = VTSS_PHY_SGMII_LANE_0_XFI;
1783     }
1784 
1785     oper_mode.xfi_pol_invert = 0;
1786     oper_mode.channel_id = VTSS_CHANNEL_AUTO;
1787 
1788     oper_mode.l_media = VTSS_MEDIA_TYPE_SR; /* Media type on the line side, based on the module used for the Line side SFP+ */
1789     //oper_mode.h_media = VTSS_MEDIA_TYPE_SR; /* Host side media type */
1790     oper_mode.l_clk_src.is_high_amp = TRUE; /* If the clock source uses a high amplitude swing this should be True. */
1791     oper_mode.xaui_lane_flip = FALSE; /* Should be set to true if the XAUI lanes are flipped when connecting to the Host */
1792     oper_mode.serdes_conf.dig_offset_reg = FALSE;
1793 
1794 #ifdef VSC8490_DEBUG
1795     GI2MIC_LOG(LOG_DEBUG, "%s : instance:%p ==> port:%d \n",__FUNCTION__,
1796                obj->inst, VSC8490_PORT);
1797 #endif
1798 
1799     rc = vtss_phy_10g_mode_set(obj->inst, VSC8490_PORT, &oper_mode);
1800     if (rc != VTSS_RC_OK) {
1801             syslog(LOG_ERR,"%s: port:%d init fail\n",__FUNCTION__, VSC8490_PORT);
1802             if (mic_link_from_ifd(obj->ifd, &dev, &link) == EOK) {
1803                     RESILIENCY_ERR_LOG(dev, MIC_ERROR_PHY_INIT_FAIL,
1804                                     "%s: %s: port:%d mode set failed\n",
1805                                     __FUNCTION__, obj->ifd->ifd_name, VSC8490_PORT);
1806             }
1807             return EFAIL;
1808     }
1809     /* platform custom initialisation */
1810     if (obj->ifd->ifd_port_type == IFDP_XSERIES_10G) {
1811         dev_addr = VSC8490_DEVAD_1;
1812         reg_addr = VSC8490_XAUI_SFI_CFG_REG1;
1813         val = GI2MIC_XAUI_SFI_CFG_REG1_VAL;
1814         rc = gi2mic_cl45_mdio_reg_write(obj, dev_addr, reg_addr, &val);
1815 
1816         reg_addr = VSC8490_XAUI_SFI_CFG_REG0;
1817         val = GI2MIC_XAUI_SFI_CFG_REG0_VAL;
1818         rc = gi2mic_cl45_mdio_reg_write(obj, dev_addr, reg_addr, &val);
1819 
1820         /* XAUI interface settings from VSC8490 PHY(port 0) to VSC3316 Switch in 10GE mode */
1821         if ( VSC8490_PORT == 0) {
1822             reg_addr = VSC8490_MII_OUTPUT_AMPL_REG;
1823             val      = GI2MIC_10G_PORT0_AMPL_VAL;
1824             dev_addr = VSC8490_DEVAD_4 ;
1825             MIC_RET_IF_ERR(gi2mic_cl45_mdio_reg_write(obj, dev_addr, reg_addr, &val));
1826             reg_addr = VSC8490_MII_POST2_REG;
1827             val      = GI2MIC_10G_PORT0_POST2_VAL;
1828             MIC_RET_IF_ERR(gi2mic_cl45_mdio_reg_write(obj, dev_addr, reg_addr, &val));
1829             reg_addr = VSC8490_MII_POST1_REG;
1830             val      = GI2MIC_10G_PORT0_POST1_VAL;
1831             MIC_RET_IF_ERR(gi2mic_cl45_mdio_reg_write(obj, dev_addr, reg_addr, &val));
1832         } else {
1833         /* XAUI interface settings from VSC8490 PHY(port 1) to IXCHIP in 10GE mode */
1834             reg_addr = VSC8490_MII_OUTPUT_AMPL_REG;
1835             val      = GI2MIC_10G_PORT1_AMPL_VAL;
1836             dev_addr = VSC8490_DEVAD_4 ;
1837             MIC_RET_IF_ERR(gi2mic_cl45_mdio_reg_write(obj, dev_addr, reg_addr, &val));
1838             reg_addr = VSC8490_MII_POST2_REG;
1839             val      = GI2MIC_10G_PORT1_POST2_VAL;
1840             MIC_RET_IF_ERR(gi2mic_cl45_mdio_reg_write(obj, dev_addr, reg_addr, &val));
1841             reg_addr = VSC8490_MII_POST1_REG;
1842             val      = GI2MIC_10G_PORT1_POST1_VAL;
1843             MIC_RET_IF_ERR(gi2mic_cl45_mdio_reg_write(obj, dev_addr, reg_addr, &val));
1844         }
1845     }
1846     gmic_slot = ((vsc8490_phy_t *)obj)->pic_slot;
1847     if (mic_link_from_ifd(obj->ifd, &dev, &link) != EOK) {
1848         GI2MIC_LOG(LOG_ERR, "%s: ifd %u not found\n",
1849                    __FUNCTION__, obj->ifd->ifd_index);
1850         return EFAIL;
1851     }
1852 
1853     if (!(PIC_IN_ISSU_WINDOW(pic))) {
1854     /* perform KATS tests in FIPS mode */
1855     if (parser_get_fips_flag()  == TRUE) {
1856         if (services_get_fips_error(gmic_slot)) {
1857             services_clear_fips_error(gmic_slot);
1858             rc = EFAIL;
1859         } else {
1860             rc = vsc8490_macsec_fips_test(obj, VSC8490_PORT);
1861         }
1862         if (rc != EOK) {
1863             services_set_fips_error(gmic_slot^1, 1);
1864             GI2MIC_LOG(LOG_ERR, "%s: %s FIPS KATS port %d failed!\n",
1865                        __FUNCTION__, ((vsc8490_phy_t *)obj)->dev_name, VSC8490_PORT);
1866             pic_set_state(obj->fpc_slot, obj->pic_slot , PIC_STATE_OFFLINE_REQ);
1867             pic_set_offline_reason(obj->fpc_slot, obj->pic_slot, REASON_SPU_FIPS_ERROR);
1868             return rc;
1869         } else {
1870         GI2MIC_LOG(LOG_INFO, "%s: %s MIC%u/PIC%u/PHY%d port %d FIPS AES-256-GCM KATS passed\n", __FUNCTION__,
1871                    obj->dev_name, obj->mic_slot,
1872                    obj->pic_slot, obj->phy_idx, obj->port_no);
1873         }
1874     } else {
1875         GI2MIC_LOG(LOG_INFO, "%s: FIPS mode not set\n", __FUNCTION__);
1876     }
1877 
1878         memset(&rxckout_conf, 0 , sizeof(vtss_phy_10g_rxckout_conf_t));
1879         rc = vtss_phy_10g_rxckout_get(obj->inst, VSC8490_PORT,
1880                                   (vtss_phy_10g_rxckout_conf_t * const)&rxckout_conf);
1881     }
1882     if (rc != EOK) {
1883         syslog(LOG_INFO, "vtss_phy_10g_txckout_get API failed\n");
1884     } else {
1885         syslog(LOG_INFO, "RX Clock Config = %d \n", rxckout_conf.mode);
1886 
1887         rxckout_conf.mode = VTSS_RECVRD_CLKOUT_LINE_SIDE_RX_CLOCK;
1888 
1889         rc = vtss_phy_10g_rxckout_set(obj->inst, VSC8490_PORT,
1890                                       &rxckout_conf);
1891         if (rc != EOK) {
1892             syslog(LOG_INFO, "%s :vtss_phy_10g_txckout_set API failed \n",
1893                    ((vsc8490_phy_t *)obj)->dev_name);
1894         } else {
1895             syslog(LOG_INFO, "%s : %s RX Clock Config = %d\n",__FUNCTION__,
1896                    ((vsc8490_phy_t *)obj)->dev_name, rxckout_conf.mode);
1897             rc = vtss_phy_10g_rxckout_get(obj->inst, VSC8490_PORT,
1898                               (vtss_phy_10g_rxckout_conf_t * const)&rxckout_conf);
1899         }
1900     }
1901 
1902     if (rc != VTSS_RC_OK) {
1903         if (mic_link_from_ifd(obj->ifd, &dev, &link) == EOK) {
1904             RESILIENCY_ERR_LOG(dev, MIC_ERROR_PHY_INIT_FAIL,
1905                   "%s: %s: port:%d init fail\n",
1906                   __FUNCTION__, obj->ifd->ifd_name, VSC8490_PORT);
1907         }
1908     }
1909     return (rc == VTSS_RC_OK) ? EOK : EFAIL;
1910 }
1911 #endif
1912 
1913 /*
1914  * ixchip mac initialisation
1915  */
1916 #ifndef IXCHIP_MAC_INIT
1917 /*
1918  *mic_gi2_mac_lnk_create
1919  * This function creates xge/ge mac handles
1920  */
1921 status_t
mic_gi2_mac_lnk_create(ifd_t * ifd)1922 mic_gi2_mac_lnk_create (ifd_t *ifd)
1923 {
1924     status_t    status = EFAIL;
1925     mic_t  *dev;
1926     int    link;
1927     lnk_mac_vector_t *ge_lnk_vec;
1928     lnk_mac_vector_t *xe_lnk_vec;
1929 
1930     if (!ifd) {
1931         GI2MIC_LOG(LOG_ERR, "%s: ifd is NULL\n", __FUNCTION__);
1932         return EFAIL;
1933     }
1934 
1935     if (mic_link_from_ifd(ifd, &dev, &link) != EOK) {
1936         GI2MIC_LOG(LOG_ERR, "%s: ifd %u not found\n",
1937                    __FUNCTION__, ifd->ifd_index);
1938         return EFAIL;
1939     }
1940 
1941     ge_lnk_vec = ix_mtip_ge_get_lnk_vector();
1942     xe_lnk_vec = ix_mtip_xe_get_lnk_vector();
1943 
1944     if(!ge_lnk_vec || !xe_lnk_vec) {
1945         GI2MIC_LOG(LOG_ERR, "%s: ifd %u ge/xe lnk vectors not found\n",
1946                    __FUNCTION__, ifd->ifd_index);
1947         return EFAIL;
1948     }
1949 
1950     if (ifd->ifd_port_type == IFDP_XSERIES_10G ) {
1951         status = xe_lnk_vec->lnk_mac_create(ifd);
1952         if (status != EOK) {
1953             GI2MIC_LOG(LOG_ERR, "%s: xge lnk create failed\n", __FUNCTION__);
1954             return status;
1955         }
1956     } else if (ifd->ifd_port_type == IFDP_XSERIES_1G ) {
1957         status = ge_lnk_vec->lnk_mac_create(ifd);
1958         if (status != EOK) {
1959             GI2MIC_LOG(LOG_ERR, "%s: ge lnk create failed\n", __FUNCTION__);
1960             return status;
1961         }
1962     } else {
1963         GI2MIC_LOG(LOG_ERR, "%s: unknown ifd,mac lnk create failed\n", __FUNCTION__);
1964         return EFAIL;
1965     }
1966 
1967     return status;
1968 }
1969 
1970 /*
1971  *mic_gi2_mac_lnk_destroy
1972  * This function creates xge/ge mac handles
1973  */
1974 status_t
mic_gi2_mac_lnk_destroy(ifd_t * ifd)1975 mic_gi2_mac_lnk_destroy (ifd_t *ifd)
1976 {
1977     status_t    status = EFAIL;
1978     mic_t  *dev;
1979     int    link;
1980     lnk_mac_vector_t *ge_lnk_vec;
1981     lnk_mac_vector_t *xe_lnk_vec;
1982 
1983     if (!ifd) {
1984         GI2MIC_LOG (LOG_ERR, "%s: ifd is NULL\n", __FUNCTION__);
1985         return EFAIL;
1986     }
1987 
1988     if (mic_link_from_ifd(ifd, &dev, &link) != EOK) {
1989         GI2MIC_LOG(LOG_ERR, "%s: ifd %u not found\n",
1990                    __FUNCTION__, ifd->ifd_index);
1991         return EFAIL;
1992     }
1993 
1994     ge_lnk_vec = ix_mtip_ge_get_lnk_vector();
1995     xe_lnk_vec = ix_mtip_xe_get_lnk_vector();
1996 
1997     if(!ge_lnk_vec || !xe_lnk_vec) {
1998         GI2MIC_LOG(LOG_ERR, "%s: ifd %u ge/xe lnk vectors not found\n",
1999                    __FUNCTION__, ifd->ifd_index);
2000         return EFAIL;
2001     }
2002 
2003     if (ifd->ifd_port_type == IFDP_XSERIES_10G ) {
2004         status = xe_lnk_vec->lnk_mac_destroy(ifd);
2005         if (status != EOK) {
2006             GI2MIC_LOG(LOG_ERR, "%s: xge lnk destroy failed\n", __FUNCTION__);
2007             return status;
2008         }
2009     } else if (ifd->ifd_port_type == IFDP_XSERIES_1G ) {
2010         status = ge_lnk_vec->lnk_mac_destroy(ifd);
2011         if (status != EOK) {
2012             GI2MIC_LOG(LOG_ERR, "%s: ge lnk destroy failed\n", __FUNCTION__);
2013             return status;
2014         }
2015     } else {
2016         GI2MIC_LOG(LOG_ERR, "%s: unknown ifd,mac lnk destroy failed\n", __FUNCTION__);
2017         return EFAIL;
2018     }
2019 
2020     return status;
2021 }
2022 
2023 /*
2024  *mic_gi2_mac_lnk_dev_init
2025  * This function creates xge/ge handle
2026  */
2027 status_t
mic_gi2_mac_lnk_dev_init(ifd_t * ifd)2028 mic_gi2_mac_lnk_dev_init (ifd_t *ifd)
2029 {
2030     status_t    status = EFAIL;
2031     mic_t  *dev;
2032     int    link;
2033     lnk_mac_vector_t *ge_lnk_vec;
2034     lnk_mac_vector_t *xe_lnk_vec;
2035 
2036     if (!ifd) {
2037         GI2MIC_LOG (LOG_ERR, "%s: ifd is NULL\n", __FUNCTION__);
2038         return EFAIL;
2039     }
2040 
2041     if (mic_link_from_ifd(ifd, &dev, &link) != EOK) {
2042         GI2MIC_LOG(LOG_ERR, "%s: ifd %u not found\n",
2043                    __FUNCTION__, ifd->ifd_index);
2044         return EFAIL;
2045     }
2046 
2047     ge_lnk_vec = ix_mtip_ge_get_lnk_vector();
2048     xe_lnk_vec = ix_mtip_xe_get_lnk_vector();
2049 
2050     if(!ge_lnk_vec || !xe_lnk_vec) {
2051         GI2MIC_LOG(LOG_ERR, "%s: ifd %u ge/xe lnk vectors not found\n",
2052                    __FUNCTION__, ifd->ifd_index);
2053         return EFAIL;
2054     }
2055 
2056     if (ifd->ifd_port_type == IFDP_XSERIES_10G ) {
2057         status = xe_lnk_vec->lnk_mac_dev_init(ifd);
2058         if (status != EOK) {
2059             GI2MIC_LOG(LOG_ERR, "%s: xge lnk dev_init failed\n", __FUNCTION__);
2060             return status;
2061         }
2062     } else if (ifd->ifd_port_type == IFDP_XSERIES_1G ) {
2063         status = ge_lnk_vec->lnk_mac_dev_init(ifd);
2064         if (status != EOK) {
2065             GI2MIC_LOG(LOG_ERR, "%s: ge lnk dev_init failed\n", __FUNCTION__);
2066             return status;
2067         }
2068     } else {
2069         GI2MIC_LOG(LOG_ERR, "%s: unknown ifd,mac lnk dev_init failed\n", __FUNCTION__);
2070         return EFAIL;
2071     }
2072 
2073     return status;
2074 }
2075 
2076 /*
2077  * mic_gi2_mac_lnk_ioctl
2078  * This function creates xge handle
2079  */
2080 status_t
mic_gi2_mac_lnk_ioctl(ifd_t * ifd,lnk_mac_ioctl_data_t * ioctl_data)2081 mic_gi2_mac_lnk_ioctl (ifd_t  *ifd,
2082                        lnk_mac_ioctl_data_t *ioctl_data)
2083 {
2084     status_t    status = EFAIL;
2085     mic_t  *dev;
2086     int    link;
2087     lnk_mac_vector_t *ge_lnk_vec;
2088     lnk_mac_vector_t *xe_lnk_vec;
2089 
2090     if (!ifd) {
2091         GI2MIC_LOG (LOG_ERR, "%s: ifd is NULL\n", __FUNCTION__);
2092         return EFAIL;
2093     }
2094 
2095     if (mic_link_from_ifd(ifd, &dev, &link) != EOK) {
2096         GI2MIC_LOG(LOG_ERR, "%s: ifd %u not found\n",
2097                    __FUNCTION__, ifd->ifd_index);
2098         return EFAIL;
2099     }
2100 
2101     ge_lnk_vec = ix_mtip_ge_get_lnk_vector();
2102     xe_lnk_vec = ix_mtip_xe_get_lnk_vector();
2103 
2104     if(!ge_lnk_vec || !xe_lnk_vec) {
2105         GI2MIC_LOG(LOG_ERR, "%s: ifd %u ge/xe lnk vectors not found\n",
2106                    __FUNCTION__, ifd->ifd_index);
2107         return EFAIL;
2108     }
2109 
2110     if (ifd->ifd_port_type == IFDP_XSERIES_10G ) {
2111         status = xe_lnk_vec->lnk_mac_ioctl(ifd, ioctl_data);
2112         if (status != EOK) {
2113             GI2MIC_LOG(LOG_ERR, "%s: xge lnk ioctl failed\n", __FUNCTION__);
2114             return status;
2115         }
2116     } else if (ifd->ifd_port_type == IFDP_XSERIES_1G ) {
2117         status = ge_lnk_vec->lnk_mac_ioctl(ifd, ioctl_data);
2118         if (status != EOK) {
2119             GI2MIC_LOG(LOG_INFO, "%s: ge lnk ioctl %d \n", __FUNCTION__,ioctl_data->cmd_type);
2120             return status;
2121         }
2122     } else {
2123         GI2MIC_LOG(LOG_ERR, "%s: unknown ifd,mac lnk ioctl failed\n", __FUNCTION__);
2124         return EFAIL;
2125     }
2126 
2127     return status;
2128 }
2129 
2130 /*
2131  *mic_gi2_mac_lnk_periodic
2132  * This function creates xge handle
2133  */
2134 status_t
mic_gi2_mac_lnk_periodic(ifd_t * ifd)2135 mic_gi2_mac_lnk_periodic (ifd_t *ifd)
2136 {
2137     status_t    status = EFAIL;
2138     mic_t  *dev;
2139     int    link;
2140     lnk_mac_vector_t *ge_lnk_vec;
2141     lnk_mac_vector_t *xe_lnk_vec;
2142 
2143     if (!ifd) {
2144         GI2MIC_LOG(LOG_ERR, "%s: ifd is NULL\n", __FUNCTION__);
2145         return EFAIL;
2146     }
2147 
2148     if (mic_link_from_ifd(ifd, &dev, &link) != EOK) {
2149         GI2MIC_LOG(LOG_ERR, "%s: ifd %u not found\n",
2150                    __FUNCTION__, ifd->ifd_index);
2151         return EFAIL;
2152     }
2153 
2154     ge_lnk_vec = ix_mtip_ge_get_lnk_vector();
2155     xe_lnk_vec = ix_mtip_xe_get_lnk_vector();
2156 
2157     if(!ge_lnk_vec || !xe_lnk_vec) {
2158         GI2MIC_LOG(LOG_ERR, "%s: ifd %u ge/xe lnk vectors not found\n",
2159                    __FUNCTION__, ifd->ifd_index);
2160         return EFAIL;
2161     }
2162 
2163     if (ifd->ifd_port_type == IFDP_XSERIES_10G ) {
2164         status = xe_lnk_vec->lnk_mac_periodic(ifd);
2165         if (status != EOK) {
2166             GI2MIC_LOG(LOG_ERR, "%s: xge lnk periodic failed\n", __FUNCTION__);
2167             RESILIENCY_ERR_LOG(dev, MIC_ERROR_PHY_RUNTIME_FAIL,
2168                   "%s: %s: xge lnk periodic failed\n",
2169                   __FUNCTION__,ifd->ifd_name);
2170             return status;
2171         }
2172     } else if (ifd->ifd_port_type == IFDP_XSERIES_1G ) {
2173         status = ge_lnk_vec->lnk_mac_periodic(ifd);
2174         if (status != EOK) {
2175             GI2MIC_LOG(LOG_ERR, "%s: ge lnk periodic failed\n", __FUNCTION__);
2176             RESILIENCY_ERR_LOG(dev, MIC_ERROR_PHY_RUNTIME_FAIL,
2177                   "%s: %s: ge lnk periodic failed\n",
2178                   __FUNCTION__,ifd->ifd_name);
2179             return status;
2180         }
2181     } else {
2182         GI2MIC_LOG(LOG_ERR, "%s: unknown ifd,mac lnk periodic failed\n", __FUNCTION__);
2183         return EFAIL;
2184     }
2185 
2186     return status;
2187 }
2188 
2189 /*
2190   * helper function to register mac vectors on Secure mic
2191   */
2192 static lnk_mac_vector_t  mic_gi2_smic_mac_vectors = {
2193      mic_gi2_mac_lnk_create,
2194      mic_gi2_mac_lnk_destroy,
2195      mic_gi2_mac_lnk_dev_init,
2196      mic_gi2_mac_lnk_ioctl,
2197      mic_gi2_mac_lnk_periodic,
2198      NULL
2199 };
2200 
2201 void *
mic_gi2_mac_get_lnk_vectors(void)2202 mic_gi2_mac_get_lnk_vectors (void)
2203 {
2204     return &mic_gi2_smic_mac_vectors;
2205 };
2206 #endif /* IXCHIP_MAC_INIT */
2207 
2208 /*
2209  * ixchip PCS initialisation
2210  */
2211 #ifndef IXCHIP_PCS_INIT
2212 /*
2213  *mic_gi2_lnk_phy_create
2214  * This function creates xge/mtip-gpcs phy handles
2215  */
2216 status_t
mic_gi2_lnk_phy_create(ifd_t * ifd)2217 mic_gi2_lnk_phy_create (ifd_t *ifd)
2218 {
2219     status_t    status = EFAIL;
2220     mic_t  *dev;
2221     int    link;
2222     lnk_phy_vector_t *lnk_phy_vec = NULL;
2223 
2224     if (!ifd) {
2225         GI2MIC_LOG(LOG_ERR, "%s: ifd is NULL\n", __FUNCTION__);
2226         return EFAIL;
2227     }
2228 
2229     if (mic_link_from_ifd(ifd, &dev, &link) != EOK) {
2230         GI2MIC_LOG(LOG_ERR, "%s: ifd %u not found\n",
2231                    __FUNCTION__, ifd->ifd_index);
2232         return EFAIL;
2233     }
2234 
2235     GI2MIC_GET_PHY_VEC(lnk_phy_vec, ifd->ifd_port_type);
2236 
2237     if(lnk_phy_vec) {
2238         status = lnk_phy_vec->lnk_phy_create(ifd);
2239         if (status != EOK) {
2240             GI2MIC_LOG(LOG_ERR, "%s: lnk phy create failed\n", __FUNCTION__);
2241             return status;
2242         }
2243     } else {
2244         GI2MIC_LOG(LOG_ERR, "%s: ifd %u ge/xe lnk phy vectors not found\n",
2245                    __FUNCTION__, ifd->ifd_index);
2246         return EFAIL;
2247     }
2248 
2249     return status;
2250 }
2251 
2252 /*
2253  *mic_gi2_lnk_phy_destroy
2254  * This function creates xge/mtip-gpcs phy handles
2255  */
2256 status_t
mic_gi2_lnk_phy_destroy(ifd_t * ifd)2257 mic_gi2_lnk_phy_destroy (ifd_t *ifd)
2258 {
2259     status_t    status = EFAIL;
2260     mic_t  *dev;
2261     int    link;
2262     lnk_phy_vector_t *lnk_phy_vec = NULL;
2263 
2264     if (!ifd) {
2265         GI2MIC_LOG(LOG_ERR, "%s: ifd is NULL\n", __FUNCTION__);
2266         return EFAIL;
2267     }
2268 
2269     if (mic_link_from_ifd(ifd, &dev, &link) != EOK) {
2270         GI2MIC_LOG(LOG_ERR, "%s: ifd %u not found\n",
2271                    __FUNCTION__, ifd->ifd_index);
2272         return EFAIL;
2273     }
2274 
2275     GI2MIC_GET_PHY_VEC(lnk_phy_vec, ifd->ifd_port_type);
2276 
2277     if(lnk_phy_vec) {
2278         status = lnk_phy_vec->lnk_phy_destroy(ifd);
2279         if (status != EOK) {
2280             GI2MIC_LOG(LOG_ERR, "%s: lnk phy create failed\n", __FUNCTION__);
2281             return status;
2282         }
2283     } else {
2284         GI2MIC_LOG(LOG_ERR, "%s: ifd %u ge/xe lnk phy vectors not found\n",
2285                    __FUNCTION__, ifd->ifd_index);
2286         return EFAIL;
2287     }
2288 
2289     return status;
2290 }
2291 
2292 /*
2293  *mic_gi2_lnk_phy_dev_init
2294  * This function creates xge/mtip-gpcs phy handle
2295  */
2296 status_t
mic_gi2_lnk_phy_dev_init(ifd_t * ifd)2297 mic_gi2_lnk_phy_dev_init (ifd_t *ifd)
2298 {
2299     status_t    status = EFAIL;
2300     mic_t  *dev;
2301     int    link;
2302     lnk_phy_vector_t *lnk_phy_vec = NULL;
2303 
2304     if (!ifd) {
2305         GI2MIC_LOG(LOG_ERR, "%s: ifd is NULL\n", __FUNCTION__);
2306         return EFAIL;
2307     }
2308 
2309     if (mic_link_from_ifd(ifd, &dev, &link) != EOK) {
2310         GI2MIC_LOG(LOG_ERR, "%s: ifd %u not found\n",
2311                    __FUNCTION__, ifd->ifd_index);
2312         return EFAIL;
2313     }
2314 
2315     GI2MIC_GET_PHY_VEC(lnk_phy_vec, ifd->ifd_port_type);
2316 
2317     if(lnk_phy_vec) {
2318         status = lnk_phy_vec->lnk_phy_dev_init(ifd);
2319         if (status != EOK) {
2320             GI2MIC_LOG(LOG_ERR, "%s: lnk phy create failed\n", __FUNCTION__);
2321             RESILIENCY_ERR_LOG(dev, MIC_ERROR_PHY_INIT_FAIL,
2322                   "%s: %slnk phy create failed\n",
2323                   __FUNCTION__,ifd->ifd_name);
2324 
2325             return status;
2326         }
2327     } else {
2328         GI2MIC_LOG(LOG_ERR, "%s: ifd %u ge/xe lnk phy vectors not found\n",
2329                    __FUNCTION__, ifd->ifd_index);
2330         return EFAIL;
2331     }
2332     return status;
2333 }
2334 
2335 /*
2336  * mic_gi2_lnk_phy_ioctl
2337  * This function creates xge handle
2338  */
2339 status_t
mic_gi2_lnk_phy_ioctl(ifd_t * ifd,lnk_phy_ioctl_data_t * ioctl_data)2340 mic_gi2_lnk_phy_ioctl (ifd_t  *ifd,
2341                        lnk_phy_ioctl_data_t *ioctl_data)
2342 {
2343     status_t    status = EFAIL;
2344     mic_t  *dev;
2345     int    link;
2346     lnk_phy_vector_t *lnk_phy_vec = NULL;
2347 
2348     if (!ifd) {
2349         GI2MIC_LOG(LOG_ERR, "%s: ifd is NULL\n", __FUNCTION__);
2350         return EFAIL;
2351     }
2352 
2353     /* In 10G mode, the phy access is handled directly by the internal phy handler */
2354     if (ifd->ifd_port_type == IFDP_XSERIES_10G) {
2355         return EOK;
2356     }
2357 
2358     if (mic_link_from_ifd(ifd, &dev, &link) != EOK) {
2359         GI2MIC_LOG(LOG_ERR, "%s: ifd %u not found\n",
2360                    __FUNCTION__, ifd->ifd_index);
2361         return EFAIL;
2362     }
2363 
2364 
2365     if ((ioctl_data->cmd_type != LNK_PHY_IOCTL_LINK_INIT) ||
2366        (ioctl_data->cmd_type != 24) ||
2367        (ioctl_data->cmd_type != 26)) {
2368     if (MIC_IN_ISSU(dev) && GI2MIC_IS_VSC8490_PORT(ifd)) {
2369         syslog(LOG_DEBUG, " IFD:%s %s: ignoring ioctl:%d call in ISSU window\n",
2370                ifd->ifd_name ,__FUNCTION__, ioctl_data->cmd_type);
2371         return EOK;
2372     }
2373     }
2374 
2375     GI2MIC_GET_PHY_VEC(lnk_phy_vec, ifd->ifd_port_type);
2376 
2377     if(lnk_phy_vec) {
2378         status = lnk_phy_vec->lnk_phy_ioctl(ifd, ioctl_data);
2379         if (status != ENOSUPPORT && status != EOK ) {
2380             GI2MIC_LOG(LOG_ERR, "%s: lnk phy ioctl failed\n", __FUNCTION__);
2381             RESILIENCY_ERR_LOG(dev, MIC_ERROR_PHY_RUNTIME_FAIL,
2382                   "%s: %s: lnk phy ioctl failed\n",
2383                   __FUNCTION__,ifd->ifd_name);
2384             return status;
2385         }
2386     } else {
2387         GI2MIC_LOG(LOG_ERR, "%s: ifd %u ge/xe lnk phy vectors not found\n",
2388                    __FUNCTION__, ifd->ifd_index);
2389         return EFAIL;
2390     }
2391 
2392     return status;
2393 }
2394 
2395 /*
2396  *mic_gi2_lnk_phy_periodic
2397  * This function creates xge handle
2398  */
2399 status_t
mic_gi2_lnk_phy_periodic(ifd_t * ifd)2400 mic_gi2_lnk_phy_periodic (ifd_t *ifd)
2401 {
2402     status_t    status;
2403     mic_t  *dev;
2404     int    link;
2405     lnk_phy_vector_t *lnk_phy_vec = NULL;
2406 
2407     if (!ifd) {
2408         GI2MIC_LOG (LOG_ERR, "%s: ifd is NULL\n", __FUNCTION__);
2409         return EFAIL;
2410     }
2411 
2412     /* In 10G mode, the phy access is handled directly by the internal phy handler */
2413     if (ifd->ifd_port_type == IFDP_XSERIES_10G) {
2414          return EOK;
2415     }
2416 
2417     if (mic_link_from_ifd(ifd, &dev, &link) != EOK) {
2418         GI2MIC_LOG(LOG_ERR, "%s: ifd %u not found\n",
2419                    __FUNCTION__, ifd->ifd_index);
2420         return EFAIL;
2421     }
2422 
2423     GI2MIC_GET_PHY_VEC(lnk_phy_vec, ifd->ifd_port_type);
2424 
2425     if(lnk_phy_vec) {
2426        status = lnk_phy_vec->lnk_phy_periodic(ifd);
2427         if (status != EOK) {
2428             GI2MIC_LOG(LOG_ERR, "%s: lnk phy periodic failed\n", __FUNCTION__);
2429             RESILIENCY_ERR_LOG(dev, MIC_ERROR_PHY_INIT_FAIL,
2430                   "%s: %s: lnk phy periodic failed\n",
2431                   __FUNCTION__,ifd->ifd_name);
2432             return status;
2433         }
2434     } else {
2435         GI2MIC_LOG(LOG_ERR, "%s: ifd %u ge/xe lnk phy vectors not found\n",
2436                    __FUNCTION__, ifd->ifd_index);
2437         return EFAIL;
2438     }
2439 
2440     return status;
2441 }
2442 
2443  /*
2444   * helper function to register mac vectors on Secure mic
2445   */
2446  static lnk_phy_vector_t  mic_gi2_smic_phy_vectors = {
2447      mic_gi2_lnk_phy_create,
2448      mic_gi2_lnk_phy_destroy,
2449      mic_gi2_lnk_phy_dev_init,
2450      mic_gi2_lnk_phy_ioctl,
2451      NULL,
2452      NULL,
2453      mic_gi2_lnk_phy_periodic,
2454      NULL
2455  };
2456 
2457  void *
mic_gi2_get_lnk_phy_vectors(void)2458  mic_gi2_get_lnk_phy_vectors (void)
2459  {
2460      return &mic_gi2_smic_phy_vectors;
2461  };
2462 #endif /* IXCHIP_PCS_INIT */
2463 
2464 /*
2465  * ixchip EXT PHY initialisation
2466  */
2467 #ifndef GI2MIC_EXT_PHY_INIT
2468 status_t
mic_gi2mic_lnk_ioctl(ifd_t * ifd,lnk_phy_ioctl_data_t * ioctl_data)2469 mic_gi2mic_lnk_ioctl (ifd_t * ifd, lnk_phy_ioctl_data_t * ioctl_data)
2470 {
2471     status_t         err = EOK;
2472     void            *obj;
2473     mic_t           *dev;
2474     int              link;
2475 
2476     /* Dont perform any i2c operation in interrupt context */
2477     if (interrupt_is_active()) {
2478 #ifdef MIC_DEBUG
2479         syslog(LOG_DEBUG, "%s: Returning since we are in interrupt context\n", __FUNCTION__);
2480 #endif
2481         return EOK;
2482     }
2483 
2484     if (ifd) {
2485         if (GI2MIC_IS_VSC8490_PORT(ifd)) {
2486             VSC8490_GET_OBJ_FROM_IFD(ifd, obj, err);
2487             if (obj && (((vsc8490_phy_t *)obj)->magic == VSC8490_MAGIC)) {
2488                 int slot = ((vsc8490_phy_t *)obj)->mic_slot;
2489                 GI2MIC_LOCK(slot);
2490                 GI2MIC_CPLD_GE_MODE_ENABLE(((vsc8490_phy_t *)obj), GE_MODE_EN);
2491                 err = vsc8490_lnk_ioctl(ifd, ioctl_data);
2492                 GI2MIC_UNLOCK(slot);
2493             }
2494         } else {
2495             VSC8584_GET_OBJ_FROM_IFD(ifd, obj, err);
2496             if (obj && (((vsc8584_phy_t *)obj)->magic == VSC8584_MAGIC)) {
2497                 int slot = ((vsc8584_phy_t *)obj)->mic_slot;
2498                 GI2MIC_LOCK(slot);
2499                 GI2MIC_CPLD_GE_MODE_ENABLE(((vsc8584_phy_t *)obj), GE_MODE_DIS);
2500                 err = vsc8584_lnk_ioctl(ifd, ioctl_data);
2501                 GI2MIC_UNLOCK(slot);
2502             }
2503         }
2504     }
2505 
2506     if (err != EOK) {
2507         syslog(LOG_ERR, "%s: mic_gi2mic_lnk_ioctl failure\n", __FUNCTION__);
2508         if (mic_link_from_ifd(ifd, &dev, &link) == EOK) {
2509             RESILIENCY_ERR_LOG(dev, MIC_ERROR_PHY_RUNTIME_FAIL,
2510                   "%s: %s: mic_gi2mic_lnk_ioctl failure\n",
2511                   __FUNCTION__,ifd->ifd_name);
2512         }
2513     }
2514     return err;
2515 }
2516 
2517 status_t
mic_gi2mic_lnk_periodic(ifd_t * ifd)2518 mic_gi2mic_lnk_periodic (ifd_t * ifd) {
2519 
2520     if (GI2MIC_IS_VSC8490_PORT(ifd)) {
2521         vsc8490_lnk_periodic(ifd);
2522     } else {
2523         vsc8584_lnk_periodic(ifd);
2524     }
2525 
2526     return EOK;
2527 }
2528 
2529 /*xge Phy vectors for Secure mic*/
2530 static lnk_phy_vector_t  mic_gi2_xge_phy_vector = {
2531         mic_phy_create,
2532         mic_phy_destroy,
2533         mic_phy_dev_init,
2534         mic_gi2mic_lnk_ioctl,
2535         NULL,
2536         mic_sfp_laser_control,
2537         mic_gi2mic_lnk_periodic,
2538         NULL
2539 };
2540 
2541 void *
mic_gi2_get_xge_phy_vector(void)2542 mic_gi2_get_xge_phy_vector(void)
2543 {
2544     return &mic_gi2_xge_phy_vector;
2545 }
2546 
2547 /* gi2mic_phy_mdio_periodic_access_test
2548  * This function performs sanity of MDIO access to PHY in mic periodic
2549  * The function tries reading the MDIO for the vendor ID information of
2550  * the PHY
2551  *
2552  * Input params - mic_t * for that MIC for which periodic is running
2553  *
2554  * Return value - status of this test - EOK on sucsess
2555  */
2556 status_t
gi2mic_phy_mdio_periodic_access_test(mic_t * dev)2557 gi2mic_phy_mdio_periodic_access_test(mic_t *dev)
2558 {
2559     status_t            res = EOK;
2560     index16_t           index = 1;
2561     vsc8584_phy_t *     handle_8584;
2562     vsc8490_phy_t *     handle_8490;
2563     u_int16_t           reg = 0x2;
2564     u_int16_t           data;
2565     u_int8_t            dev_id = 0x1;
2566     pic_t               *pic = dev->pic_context;
2567     level_t             level;
2568 
2569     /* Sanity check  of dev pointer */
2570     if (!dev) {
2571 #ifdef VSC8584_DEBUG
2572         syslog (LOG_ERR, "%s: dev pointer NULL\n", __FUNCTION__);
2573 #endif
2574         return EINVAL;
2575     }
2576 
2577     level = interrupt_raise_priority(LEVEL_NETS);
2578 
2579     if (pic->dynamic_mode != JAM_CH_PIC_MODE_10G ) {
2580 #ifdef VSC8584_DEBUG
2581         syslog (LOG_DEBUG, "%s: dynamic_mode != 10G-only mode\n",
2582                            __FUNCTION__);
2583 #endif
2584         /* Read 1st index value for VSC8584 */
2585         handle_8584 = vsc8584_itable16_lookup(index);
2586 
2587         if (handle_8584 != NULL && handle_8584->itable_index == index) {
2588 #ifdef VSC8584_DEBUG
2589             syslog(LOG_DEBUG, "%s: VSC8584 device handle obtained for index %d\n",
2590                                __FUNCTION__, index);
2591 #endif
2592 
2593             res = vsc8584_reg_read (handle_8584, 0,0, reg, &data);
2594             if(res != EOK) {
2595                 interrupt_restore_priority(level);
2596                 syslog(LOG_ERR, "%s: VSC8584 MDIO read of Vendor ID failed\n",
2597                                 __FUNCTION__);
2598                 return res;
2599             } else {
2600 #ifdef VSC8584_DEBUG
2601                 syslog(LOG_DEBUG, "%s: VSC8584 MDIO read of Vendor ID returned %x\n",
2602                                   __FUNCTION__, data);
2603 #endif
2604             }
2605 
2606             if (data != VSC8584_VENDOR_ID) {
2607                 interrupt_restore_priority(level);
2608                 syslog(LOG_ERR, "%s: Wrong VSC8584 Vendor ID value fetched"
2609                                 " from MDIO read - expected = 0x7, actual = %x\n",
2610                                  __FUNCTION__, data);
2611                 return EINVAL;
2612             }
2613         } else {
2614 #ifdef VSC8584_DEBUG
2615             syslog(LOG_ERR, "%s: VSC8584 device handle unavailable\n",
2616                             __FUNCTION__);
2617 #endif
2618         }
2619 
2620         /* Read 1st index value for VSC8490 */
2621         handle_8490 = vsc8490_itable16_lookup(index);
2622 
2623         if (handle_8490 != NULL && handle_8490->itable_index == index) {
2624 #ifdef VSC8490_DEBUG
2625             syslog(LOG_DEBUG, "%s: VSC8490 device handle obtained for index %d\n",
2626                               __FUNCTION__, index);
2627 #endif
2628 
2629             res = gi2mic_cl45_reg_read(handle_8490, dev_id, reg, &data);
2630             if(res != EOK) {
2631                 interrupt_restore_priority(level);
2632                 syslog(LOG_ERR, "%s: VSC8490 MDIO read of Vendor ID failed\n",
2633                                  __FUNCTION__);
2634                 return res;
2635             } else {
2636 #ifdef VSC8490_DEBUG
2637                 syslog(LOG_DEBUG, "%s: VSC8490 MDIO read of Vendor ID returned %x\n",
2638                                   __FUNCTION__, data);
2639 #endif
2640             }
2641 
2642             if (data != VSC8490_VENDOR_ID) {
2643                 interrupt_restore_priority(level);
2644                 syslog(LOG_ERR, "%s: Wrong VSC8490 Vendor ID value fetched"
2645                                 " from MDIO read - expected = 0x7, actual = %x\n",
2646                                  __FUNCTION__, data);
2647                 return EINVAL;
2648             }
2649         } else {
2650 #ifdef VSC8490_DEBUG
2651             syslog(LOG_ERR, "%s: VSC8490 device handle unavailable\n",
2652                             __FUNCTION__);
2653 #endif
2654         }
2655     } else {
2656         /* We are in JAM_CH_PIC_MODE_10G mode */
2657 #ifdef VSC8490_DEBUG
2658         syslog (LOG_DEBUG, "%s: dynamic_mode = 10G mode\n", __FUNCTION__);
2659 #endif
2660 
2661         /* Read 1st index value for VSC8490 */
2662         handle_8490 = vsc8490_itable16_lookup(index);
2663 
2664         if (handle_8490 != NULL && handle_8490->itable_index == index) {
2665 #ifdef VSC8490_DEBUG
2666             syslog(LOG_DEBUG, "%s: VSC8490 device handle obtained for index %d\n",
2667                               __FUNCTION__, index);
2668 #endif
2669 
2670             res = gi2mic_cl45_reg_read(handle_8490, dev_id, reg, &data);
2671             if(res != EOK) {
2672                 interrupt_restore_priority(level);
2673                 syslog(LOG_ERR, "%s: VSC8490 MDIO read of Vendor ID failed\n",
2674                                 __FUNCTION__);
2675                 return res;
2676             } else {
2677 #ifdef VSC8490_DEBUG
2678                 syslog(LOG_DEBUG, "%s: VSC8490 MDIO read of Vendor ID returned %x\n",
2679                                   __FUNCTION__, data);
2680 #endif
2681             }
2682 
2683             if (data != VSC8490_VENDOR_ID) {
2684                 interrupt_restore_priority(level);
2685                 syslog(LOG_ERR, "%s: Wrong VSC8490 Vendor ID value fetched"
2686                                 " from MDIO read - expected = 0x7, actual = %x\n",
2687                                  __FUNCTION__, data);
2688                 return EINVAL;
2689             }
2690         } else {
2691 #ifdef VSC8490_DEBUG
2692             syslog(LOG_ERR, "%s: VSC8490 device handle unavailable\n",
2693                             __FUNCTION__);
2694 #endif
2695         }
2696     }
2697     interrupt_restore_priority(level);
2698 
2699     return res;
2700 }
2701 
2702 /* gi2mic_periodic_access_sanity_check
2703  * This function performs sanity of TX clock, accesses to PHY, MDIO etc.
2704  * in mic periodic.
2705  *
2706  * Input params - mic_t * for that MIC for which periodic is running
2707  *
2708  * Return value - status of this test - EOK on sucsess
2709  */
2710 status_t
gi2mic_periodic_access_sanity_check(mic_t * dev)2711 gi2mic_periodic_access_sanity_check(mic_t *dev)
2712 {
2713     status_t  status = EFAIL;
2714     u_int8_t  data   = 0x0;
2715 
2716     /* First read the TX Clock buffer register */
2717     status = mic_mezz_i2cs_reg_rd (dev, GI2MIC_MEZZ_I2CS_TX_CLK_BUFF_ADDR, 1, &data);
2718 
2719     /* Check for bit 0 of the TX Clock buffer register to determine clock */
2720     if (!(data & GI2MIC_SI53302_LOS0)) {
2721 #ifdef MIC_DEBUG
2722         syslog(LOG_DEBUG, "%s: TX Clock buffer is ok for MIC %s\n",
2723                            __FUNCTION__, dev->pic_name);
2724 #endif
2725 
2726         data = 0x0; /* Clear the data read */
2727 
2728         /* TX Clock buffer is ok, now read the TX PLL status register */
2729         status = mic_mezz_i2cs_reg_rd (dev, GI2MIC_MEZZ_I2CS_SI5348_STATUS_REG_ADDR, 1, &data);
2730 
2731         /* Check for bit 0, 1 and 2 for TX PLL signals */
2732         if ((data & 0x7) == 0x0) {
2733 #ifdef MIC_DEBUG
2734             syslog(LOG_DEBUG, "%s: TX PLL is locked for %s\n",
2735                                __FUNCTION__, dev->pic_name);
2736 #endif
2737         } else {
2738             syslog(LOG_INFO, "%s: TX PLL not locked for %s yet\n",
2739                               __FUNCTION__, dev->pic_name);
2740             syslog(LOG_INFO, "%s: TX PLL lock status register dump for reference: "
2741                              " MEZZ_I2CS_SI5348_STATUS_REG_ADDR = 0x25, value = 0x%x\n",
2742                               __FUNCTION__, data);
2743         }
2744     } else {
2745         syslog(LOG_ERR, "%s: TX Clock Buffer failed - no TX clock available "
2746                         "for %s, exiting periodic\n",
2747                         __FUNCTION__, dev->pic_name);
2748         syslog(LOG_INFO, "%s: TX Clock Buffer Register dump: "
2749                          "MEZZ_I2CS_TX_CLK_BUFF_ADDR = 0x2C,  value = 0x%x\n",
2750                           __FUNCTION__, data);
2751         return status;
2752     }
2753 
2754 
2755     /* Do MDIO read of PHY Vendor ID register for MDIO sanity */
2756     status = gi2mic_phy_mdio_periodic_access_test(dev);
2757     if (status != EOK) {
2758         syslog(LOG_ERR,"%s: MDIO access failure reported in %s\n",
2759                        __FUNCTION__, dev->pic_name);
2760         RESILIENCY_ERR_LOG(dev, MIC_ERROR_PHY_RUNTIME_FAIL,
2761                   "%s: MDIO access failure reported in %s\n",
2762                   __FUNCTION__, dev->pic_name);
2763         return status;
2764     }
2765 
2766 #ifdef ENABLE_PCI_CHECK
2767     /* Read vendor ID for IXCHIP for PCI sanity */
2768     status = mic_ix_read_pci_vendor_id(dev);
2769     if (status != EOK) {
2770         syslog(LOG_ERR,"%s: PCI access failure reported in MIC %s,"
2771                        " exiting periodic\n",
2772                        __FUNCTION__, dev->pic_name);
2773         return status;
2774     }
2775 #endif
2776 
2777     return status;
2778 }
2779 
2780 /*
2781  * gi2mic_macsec_init
2782  *
2783  * This function will initialize the MACsec related structures in the PHY.
2784  */
2785 static status_t
gi2mic_macsec_init(void)2786 gi2mic_macsec_init (void)
2787 {
2788 
2789 /*
2790  * Nothing to be done by way of init for GI2MIC macsec PHYs
2791  * TODO: Check if global flag for macsec init needs to be added?
2792  */
2793 
2794     return TRUE;
2795 }
2796 
2797 /*
2798  * gi2mic_macsec_port_reinit
2799  *
2800  * This function enables or disables MACsec for a port based on configuration.
2801  *
2802  */
2803 static status_t
gi2mic_macsec_port_reinit(pic_t * pic,ifd_t * ifd,boolean is_enable)2804 gi2mic_macsec_port_reinit (pic_t *pic, ifd_t *ifd, boolean is_enable)
2805 {
2806     syslog(LOG_INFO, "%s %d: STUB function, returning EOK\n", __FUNCTION__, __LINE__);
2807     return EOK;
2808 }
2809 
2810 /*
2811  * gi2mic_macsec_tx_sc_create
2812  *
2813  * This function calls the generic secure channel creation api for TX
2814  * with gi2mic specifics handled.
2815  */
2816 static status_t
gi2mic_macsec_tx_sc_create(ifd_t * ifd,macsec_tx_sc_config_t * config)2817 gi2mic_macsec_tx_sc_create (ifd_t *ifd, macsec_tx_sc_config_t *config)
2818 {
2819     if (GI2MIC_IS_VSC8490_PORT(ifd)) {
2820         return (vsc8490_macsec_tx_sc_create(ifd, config));
2821     } else {
2822         return (vsc8584_macsec_tx_sc_create(ifd, config));
2823     }
2824 }
2825 
2826 /*
2827  * gi2mic_macsec_rx_sc_create
2828  *
2829  * This function calls the generic secure channel creation api for RX
2830  * with gi2mic specifics handled.
2831  */
2832 static status_t
gi2mic_macsec_rx_sc_create(ifd_t * ifd,macsec_rx_sc_config_t * config)2833 gi2mic_macsec_rx_sc_create (ifd_t *ifd, macsec_rx_sc_config_t *config)
2834 {
2835     if (GI2MIC_IS_VSC8490_PORT(ifd)) {
2836         return (vsc8490_macsec_rx_sc_create(ifd, config));
2837     } else {
2838         return (vsc8584_macsec_rx_sc_create(ifd, config));
2839     }
2840 }
2841 
2842 /*
2843  * gi2mic_macsec_sc_delete
2844  *
2845  * This function calls the generic secure channel deletion api
2846  * with gi2mic specifics handled.
2847  */
2848 static status_t
gi2mic_macsec_sc_delete(ifd_t * ifd,uint8_t chan_id)2849 gi2mic_macsec_sc_delete (ifd_t *ifd, uint8_t chan_id)
2850 {
2851     if (GI2MIC_IS_VSC8490_PORT(ifd)) {
2852         return (vsc8490_macsec_sc_delete(ifd, chan_id));
2853     } else {
2854         return (vsc8584_macsec_sc_delete(ifd, chan_id));
2855     }
2856 }
2857 
2858 /*
2859  * gi2mic_macsec_sa_create
2860  *
2861  * This function calls the generic secure association create api
2862  * with gi2mic specifics handled.
2863  */
2864 static status_t
gi2mic_macsec_sa_create(ifd_t * ifd,u_int8_t sc_index,int8_t * sa_id,u_int32_t sa_num,u_int32_t next_pn,u_int8_t key[16],u_int8_t offset,u_int32_t flag)2865 gi2mic_macsec_sa_create (ifd_t *ifd, u_int8_t sc_index, int8_t *sa_id,
2866                       u_int32_t sa_num, u_int32_t next_pn, u_int8_t key[16],
2867                       u_int8_t offset, u_int32_t flag)
2868 {
2869     if (GI2MIC_IS_VSC8490_PORT(ifd)) {
2870         return (vsc8490_macsec_sa_create(ifd, sc_index, sa_id,
2871                                          sa_num, next_pn, key, offset, flag));
2872     } else {
2873         return (vsc8584_macsec_sa_create(ifd, sc_index, sa_id, sa_num, next_pn,
2874                                          key, offset, flag));
2875     }
2876 }
2877 
2878 /*
2879  * gi2mic_macsec_tx_flow_create
2880  *
2881  * This function calls the generic flow creation api for TX
2882  * with gi2mic specifics handled.
2883  */
2884 static status_t
gi2mic_macsec_tx_flow_create(ifd_t * ifd,macsec_flow_config_t * config)2885 gi2mic_macsec_tx_flow_create (ifd_t *ifd, macsec_flow_config_t *config)
2886 {
2887     if (GI2MIC_IS_VSC8490_PORT(ifd)) {
2888         return (vsc8490_macsec_tx_flow_create(ifd, config));
2889     } else {
2890         return (vsc8584_macsec_tx_flow_create(ifd, config));
2891     }
2892 }
2893 
2894 /*
2895  * gi2mic_macsec_rx_flow_create
2896  *
2897  * This function calls the generic flow creation api for RX
2898  * with gi2mic specifics handled.
2899  */
2900 static status_t
gi2mic_macsec_rx_flow_create(ifd_t * ifd,macsec_flow_config_t * config)2901 gi2mic_macsec_rx_flow_create (ifd_t *ifd, macsec_flow_config_t *config)
2902 {
2903     if (GI2MIC_IS_VSC8490_PORT(ifd)) {
2904         return (vsc8490_macsec_rx_flow_create(ifd, config));
2905     } else {
2906         return (vsc8584_macsec_rx_flow_create(ifd, config));
2907     }
2908 }
2909 
2910 /*
2911  * gi2mic_macsec_flow_delete
2912  *
2913  * This function calls the generic flow deletion api
2914  * with gi2mic specifics handled.
2915  */
2916 static status_t
gi2mic_macsec_flow_delete(ifd_t * ifd,macsec_flow_config_t * config)2917 gi2mic_macsec_flow_delete (ifd_t *ifd, macsec_flow_config_t *config)
2918 {
2919     if (GI2MIC_IS_VSC8490_PORT(ifd)) {
2920         return (vsc8490_macsec_flow_delete(ifd, config));
2921     } else {
2922         return (vsc8584_macsec_flow_delete(ifd, config));
2923     }
2924 }
2925 
2926 /*
2927  * gi2mic_macsec_block_enable
2928  *
2929  * This function calls the generic macsec block enable api for a port
2930  * with gi2mic specifics handled.
2931  */
2932 static status_t
gi2mic_macsec_block_enable(macsec_ifd_ifl_t * macsec_ifd_ifl,boolean fail_action)2933 gi2mic_macsec_block_enable (macsec_ifd_ifl_t *macsec_ifd_ifl,
2934                             boolean fail_action)
2935 {
2936     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
2937         return (vsc8490_macsec_block_enable(macsec_ifd_ifl, fail_action));
2938     } else {
2939         return (vsc8584_macsec_block_enable(macsec_ifd_ifl, fail_action));
2940     }
2941 }
2942 
2943 /*
2944  * gi2mic_macsec_block_disable
2945  *
2946  * This function calls the generic macsec block disable api for a port
2947  * with gi2mic specifics handled.
2948  */
2949 static status_t
gi2mic_macsec_block_disable(macsec_ifd_ifl_t * macsec_ifd_ifl)2950 gi2mic_macsec_block_disable (macsec_ifd_ifl_t *macsec_ifd_ifl)
2951 {
2952     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
2953         return (vsc8490_macsec_block_disable(macsec_ifd_ifl));
2954     } else {
2955         return (vsc8584_macsec_block_disable(macsec_ifd_ifl));
2956     }
2957 }
2958 
2959 /*
2960  * gi2mic_get_macsec_ifl_stats
2961  *
2962  * This function calls the generic macsec statistics collection api for a ifl
2963  * with gi2mic specifics handled.
2964  */
2965 static status_t
gi2mic_get_macsec_ifl_stats(ifd_t * ifd,macsec_ifl_secy_node_t * secy_node,macsec_stats_t * stats)2966 gi2mic_get_macsec_ifl_stats (ifd_t *ifd, macsec_ifl_secy_node_t *secy_node,
2967                              macsec_stats_t *stats)
2968 {
2969     if (GI2MIC_IS_VSC8490_PORT(ifd)) {
2970         return (vsc8490_get_macsec_ifl_stats(ifd, secy_node, stats));
2971     } else {
2972         return (vsc8584_get_macsec_ifl_stats(ifd, secy_node, stats));
2973     }
2974 }
2975 
2976 /*
2977  * gi2mic_get_macsec_stats
2978  *
2979  * This function calls the generic macsec statistics collection api for a port
2980  * with gi2mic specifics handled.
2981  */
2982 static status_t
gi2mic_get_macsec_stats(ifd_t * ifd,macsec_stats_t * stats)2983 gi2mic_get_macsec_stats (ifd_t *ifd, macsec_stats_t *stats)
2984 {
2985     if (GI2MIC_IS_VSC8490_PORT(ifd)) {
2986         return (vsc8490_get_macsec_stats(ifd, stats));
2987     } else {
2988         return (vsc8584_get_macsec_stats(ifd, stats));
2989     }
2990 }
2991 
2992 /*
2993  * gi2mic_macsec_update_addr
2994  *
2995  * This function updates the macsec phy address in the pic structure to help
2996  * in checking the events triggered for these addresses.
2997  */
2998 static status_t
gi2mic_macsec_update_addr(pic_t * pic,ifd_t * ifd,boolean is_add)2999 gi2mic_macsec_update_addr (pic_t *pic, ifd_t *ifd, boolean is_add)
3000 {
3001     if (GI2MIC_IS_VSC8490_PORT(ifd)) {
3002         return (vsc8490_macsec_update_addr(pic, ifd, is_add));
3003     } else {
3004         return (vsc8584_macsec_update_addr(pic, ifd, is_add));
3005     }
3006 }
3007 
3008 /*
3009  * gi2mic_macsec_event_handler
3010  *
3011  * This function checks for any events for the macsec addresses
3012  */
3013 static void
gi2mic_macsec_event_handler(pic_t * pic)3014 gi2mic_macsec_event_handler (pic_t* pic)
3015 {
3016     return;
3017 }
3018 
3019 static status_t
gi2mic_macsec_secy_create(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_secy_config_t * secy_config)3020 gi2mic_macsec_secy_create (macsec_ifd_ifl_t *macsec_ifd_ifl,
3021                            macsec_secy_config_t *secy_config)
3022 {
3023     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
3024         return (vsc8490_macsec_secy_add(macsec_ifd_ifl, secy_config));
3025     } else {
3026         return (vsc8584_macsec_secy_add(macsec_ifd_ifl, secy_config));
3027     }
3028 }
3029 
3030 static status_t
gi2mic_macsec_tx_sa_create(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_tx_sa_config_t * tx_sa_config)3031 gi2mic_macsec_tx_sa_create (macsec_ifd_ifl_t *macsec_ifd_ifl,
3032                             macsec_tx_sa_config_t *tx_sa_config)
3033 {
3034     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
3035         return (vsc8490_macsec_tx_sa_create(macsec_ifd_ifl, tx_sa_config));
3036     } else {
3037         return (vsc8584_macsec_tx_sa_create(macsec_ifd_ifl, tx_sa_config));
3038     }
3039 }
3040 
3041 static status_t
gi2mic_macsec_rx_sa_create(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_rx_sa_config_t * config)3042 gi2mic_macsec_rx_sa_create(macsec_ifd_ifl_t *macsec_ifd_ifl,
3043                            macsec_rx_sa_config_t *config)
3044 {
3045     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
3046         return (vsc8490_macsec_rx_sa_create(macsec_ifd_ifl, config));
3047     } else {
3048         return (vsc8584_macsec_rx_sa_create(macsec_ifd_ifl, config));
3049     }
3050 }
3051 
3052 static status_t
gi2mic_macsec_tx_activate(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_secy_id_t * secy_id,uint8_t * config_sci __unused,u_int16_t an)3053 gi2mic_macsec_tx_activate (macsec_ifd_ifl_t *macsec_ifd_ifl,
3054                            macsec_secy_id_t *secy_id,
3055                            uint8_t *config_sci __unused,
3056                            u_int16_t an)
3057 {
3058     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
3059         return (vsc8490_macsec_tx_activate(macsec_ifd_ifl, secy_id,
3060                                            config_sci, an));
3061     } else {
3062         return (vsc8584_macsec_tx_activate(macsec_ifd_ifl, secy_id,
3063                                            config_sci, an));
3064     }
3065 }
3066 
3067 static status_t
gi2mic_macsec_rx_activate(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_secy_id_t * secy_id,uint8_t * config_sci,u_int16_t an)3068 gi2mic_macsec_rx_activate (macsec_ifd_ifl_t *macsec_ifd_ifl,
3069                            macsec_secy_id_t *secy_id,
3070                            uint8_t *config_sci,
3071                            u_int16_t an)
3072 {
3073     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
3074         return (vsc8490_macsec_rx_activate(macsec_ifd_ifl, secy_id,
3075                                            config_sci, an));
3076     } else {
3077         return (vsc8584_macsec_rx_activate(macsec_ifd_ifl, secy_id,
3078                                            config_sci, an));
3079     }
3080 
3081 
3082 }
3083 
3084 static status_t
gi2mic_macsec_secy_delete(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_secy_id_t * secy_id)3085 gi2mic_macsec_secy_delete(macsec_ifd_ifl_t *macsec_ifd_ifl,
3086                           macsec_secy_id_t *secy_id)
3087 {
3088     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
3089         return (vsc8490_macsec_secy_delete(macsec_ifd_ifl, secy_id));
3090     } else {
3091         return (vsc8584_macsec_secy_delete(macsec_ifd_ifl, secy_id));
3092     }
3093 }
3094 
3095 static status_t
gi2mic_macsec_tx_sc_delete(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_secy_id_t * secy_id,uint8_t * tx_sci __unused)3096 gi2mic_macsec_tx_sc_delete (macsec_ifd_ifl_t *macsec_ifd_ifl,
3097                             macsec_secy_id_t *secy_id, uint8_t *tx_sci __unused)
3098 {
3099     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
3100         return (vsc8490_macsec_tx_sc_delete(macsec_ifd_ifl, secy_id, tx_sci));
3101     } else {
3102         return (vsc8584_macsec_tx_sc_delete(macsec_ifd_ifl, secy_id, tx_sci));
3103     }
3104 }
3105 
3106 static status_t
gi2mic_macsec_rx_sc_delete(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_secy_id_t * secy_id,uint8_t * rx_sci __unused)3107 gi2mic_macsec_rx_sc_delete (macsec_ifd_ifl_t *macsec_ifd_ifl,
3108                             macsec_secy_id_t *secy_id, uint8_t *rx_sci __unused)
3109 {
3110     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
3111         return (vsc8490_macsec_rx_sc_delete(macsec_ifd_ifl, secy_id, rx_sci));
3112     } else {
3113         return (vsc8584_macsec_rx_sc_delete(macsec_ifd_ifl, secy_id, rx_sci));
3114     }
3115 }
3116 
3117 static status_t
gi2mic_macsec_control_flow_create(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_flow_config_t * flow_config)3118 gi2mic_macsec_control_flow_create (macsec_ifd_ifl_t *macsec_ifd_ifl,
3119                                    macsec_flow_config_t *flow_config)
3120 {
3121     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
3122         return (vsc8490_macsec_control_flow_create(macsec_ifd_ifl,
3123                                                    flow_config));
3124     } else {
3125         return (vsc8584_macsec_control_flow_create(macsec_ifd_ifl,
3126                                                    flow_config));
3127     }
3128 }
3129 
3130 /*
3131  * gi2mic_macsec_ifl_supported
3132  *
3133  * This function returns TRUE if IFL MACsec is supported.
3134  */
3135 static boolean
gi2mic_macsec_ifl_supported(void)3136 gi2mic_macsec_ifl_supported (void)
3137 {
3138     return TRUE;
3139 }
3140 
3141 /*
3142  * gi2mic_macsec_init_post_issu
3143  *
3144  * This function prepares the VSC SDK for the macsec configuration.
3145  * It is called after ISSU completes on a FPC.
3146  */
3147 static void
gi2mic_macsec_init_post_issu(u_int32_t fpc_slot,pic_t * pic)3148 gi2mic_macsec_init_post_issu (u_int32_t fpc_slot, pic_t *pic)
3149 {
3150     return;
3151 }
3152 
3153 static status_t
gi2mic_macsec_control_flow_delete(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_flow_config_t * flow_config)3154 gi2mic_macsec_control_flow_delete (macsec_ifd_ifl_t *macsec_ifd_ifl,
3155                                    macsec_flow_config_t *flow_config)
3156 {
3157     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
3158         return (vsc8490_macsec_control_flow_delete(macsec_ifd_ifl, flow_config));
3159     } else {
3160         return (vsc8584_macsec_control_flow_delete(macsec_ifd_ifl, flow_config));
3161     }
3162 }
3163 /*
3164  * gi2mic_macsec_rx_sa_lowest_pn_update
3165  *
3166  * This function updates the VSC SDK with the lowest acceptable
3167  * packet number value as received from peer MKA packet when
3168  * bounded delay protect feature is enabled.
3169  */
3170 static status_t
gi2mic_macsec_rx_sa_lowest_pn_update(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_secy_id_t * secy_id,uint8_t * sci,uint16_t an,uint64_t llpn)3171 gi2mic_macsec_rx_sa_lowest_pn_update (macsec_ifd_ifl_t *macsec_ifd_ifl,
3172                                       macsec_secy_id_t *secy_id,
3173                                       uint8_t *sci, uint16_t an,
3174                                       uint64_t llpn)
3175 {
3176 
3177     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
3178         return (vsc8490_macsec_rx_sa_lowest_pn_update(macsec_ifd_ifl,
3179                                                       secy_id,
3180                                                       sci,an,
3181                                                       llpn));
3182     } else {
3183         return (vsc8584_macsec_rx_sa_lowest_pn_update(macsec_ifd_ifl,
3184                                                       secy_id,
3185                                                       sci,an,
3186                                                       llpn));
3187     }
3188 }
3189 static void
vsc8490_macsec_tx_lapn_thread_fn(void)3190 vsc8490_macsec_tx_lapn_thread_fn (void)
3191 {
3192     wakeup_trigger_t                    trigger;
3193     void                                *trigger_context;
3194     timer_t                             tx_llpn_poll_timer;
3195     vsc8490_macsec_bounded_delay_node_t         *run;
3196     status_t                            status = EOK;
3197     ms_ciphersuite_t                    cipher;
3198     macsec_secy_id_t                    secy;
3199     ifd_t                               *ifd = NULL;
3200     ifl_t                               *ifl = NULL;
3201     ifd_macsec_t                        *macsec_settings = NULL;
3202 
3203     timer_init_leaf(&tx_llpn_poll_timer, NULL, 0);
3204     timer_start(&tx_llpn_poll_timer,VSC8490_MACSEC_TX_LAPN_POLL_TIMER_MS);
3205     vsc8490_bd_thread_start_time = time_get_ms();
3206 
3207     while (TRUE) {
3208         thread_sleep();
3209         trigger_context = wakeup_trigger_fetch(&trigger);
3210         switch (trigger.type) {
3211             case WAKEUP_TYPE_TIMER:
3212                 timer_start(&tx_llpn_poll_timer, VSC8490_MACSEC_TX_LAPN_POLL_TIMER_MS);
3213                     run = vsc8490_bounded_delay_interface_list.first;
3214                     while (run != NULL) {
3215                         ifd  = run->ifd;
3216                         ifl = run->ifl;
3217                         if (run->is_ifl_macsec && run->is_ifl_macsec != MACSEC_INVALID_VALUE) {
3218                             /* Polling IFL stats get */
3219                             if (ifl) {
3220                                 macsec_settings  = ifl->macsec_settings;
3221                                 cipher = ifd->macsec_settings->macsec_secy[0].ciphersuite;
3222                                 secy = run->secy_id;
3223                                 status = vsc8490_macsec_nextpn_stats_get(ifd,
3224                                                                      secy.secy_vport_id,
3225                                                                      secy.secy_service_id,
3226                                                                     &macsec_settings->macsec_stats, cipher);
3227                                 if (status != EOK) {
3228                                     syslog(LOG_ERR,"%s:vsc8490_macsec_stats_get:status Error :%d \n",
3229                                            __FUNCTION__,status);
3230                                 }
3231                             } else {
3232                                 syslog(LOG_ERR,"%s:IFL is NUll \n",__FUNCTION__);
3233                             }
3234 
3235                         } else {
3236                             /* Polling IFD stats */
3237                             if (ifd) {
3238                                 macsec_settings  = ifd->macsec_settings;
3239                                 cipher = ifd->macsec_settings->macsec_secy[0].ciphersuite;
3240                                 secy = run->secy_id;
3241                                 status = vsc8490_macsec_nextpn_stats_get(ifd,
3242                                                  secy.secy_vport_id,
3243                                                  secy.secy_service_id,
3244                                                  &macsec_settings->macsec_stats, cipher);
3245                                 if (status != EOK) {
3246                                     syslog(LOG_ERR,"%s:vsc8490_macsec_stats_get:status Error :%d \n",
3247                                            __FUNCTION__,status);
3248                                 }
3249                              } else {
3250                                 syslog(LOG_ERR, "%s: NULL ifd \n",__FUNCTION__);
3251                              }
3252                         }
3253                         run = run->next;
3254                     }/* End of while loop */
3255             break;
3256             default:
3257                 wakeup_trigger_error(&trigger);
3258          }
3259     }
3260 }
3261 static status_t
gi2mic_vsc8490_macsec_tx_sa_lowest_pn_poll(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_secy_id_t * secy_id,boolean is_opAdd)3262 gi2mic_vsc8490_macsec_tx_sa_lowest_pn_poll (macsec_ifd_ifl_t *macsec_ifd_ifl,
3263                                           macsec_secy_id_t *secy_id,
3264                                           boolean is_opAdd)
3265 {
3266     status_t                            status = EOK;
3267     ifd_t                               *ifd = NULL;
3268     ifl_t                               *ifl = NULL;
3269     ifd_macsec_t                        *macsec_settings = NULL;
3270     vsc8490_macsec_bounded_delay_node_t         *node;
3271     vsc8490_macsec_bounded_delay_node_t         *run;
3272     vsc8490_macsec_bounded_delay_node_t         *prev;
3273     int                                 i = 0;
3274     thread_t                            *thread;
3275 
3276     MACSEC_GET_MACSEC_SETTINGS(macsec_ifd_ifl);
3277     if (macsec_ifd_ifl->is_ifl_macsec) {
3278         ifl = macsec_ifd_ifl->ifl;
3279         MACSEC_NULL_PTR_CHECK_RETURN_STATUS(ifl, "ifl is NULL");
3280     }else{
3281          syslog(LOG_INFO, "%s: ifd %d (%s) is_opAdd:%d \n",__FUNCTION__,
3282                 ifd->ifd_index, ifd->ifd_name, is_opAdd);
3283     }
3284     syslog(LOG_INFO,"%s: secy vport id: %d  is_bounded_delay_enable: %d \n",__FUNCTION__,
3285            secy_id->secy_vport_id, macsec_settings->is_bounded_delay_enable );
3286 
3287     if ( (vsc8490_bounded_delay_interface_count == VSC8490_MAX_SUPPORTED_BOUNDED_DELAY_INTERAFACE) &&
3288         is_opAdd ){
3289         syslog(LOG_ERR,"%s: Discarding as exceeding max supported:: %d \n",
3290                __FUNCTION__, vsc8490_bounded_delay_interface_count);
3291         return status;
3292     }
3293 
3294 
3295     if ( (macsec_settings->is_bounded_delay_enable) && is_opAdd ) {
3296         /* Add to data structure */
3297         node = (vsc8490_macsec_bounded_delay_node_t *)kzalloc(sizeof(vsc8490_macsec_bounded_delay_node_t));
3298         if (node == NULL) {
3299             syslog(LOG_ERR, "%s: Failed to allocate memory for macsec_bounded_delay_node \n",__FUNCTION__);
3300             status = EFAIL;
3301             return status;
3302         }
3303         node->secy_id.ifd_index       = secy_id->ifd_index;
3304         node->secy_id.secy_vport_id   = secy_id->secy_vport_id;
3305         node->secy_id.secy_service_id = secy_id->secy_service_id;
3306         node->secy_id.encryption      = secy_id->encryption;
3307         node->is_ifl_macsec           = macsec_ifd_ifl->is_ifl_macsec;
3308 
3309         if (macsec_ifd_ifl->is_ifl_macsec) {
3310             syslog(LOG_INFO, "%s: Populating node list ifl , vport id :%d  is_opAdd:%d \n",
3311                    __FUNCTION__, node->secy_id.secy_vport_id, is_opAdd);
3312             node->ifl                     = ifl;
3313             node->ifd                     = ifd;
3314         }else{
3315             syslog(LOG_INFO, "%s: Populating node list ifd %d (%s) is_opAdd:%d \n",
3316                    __FUNCTION__, ifd->ifd_index, ifd->ifd_name, is_opAdd);
3317             node->ifd                     = ifd;
3318             node->ifl                     = NULL;
3319         }
3320 
3321         if (vsc8490_bounded_delay_interface_list.first == NULL ){
3322             syslog(LOG_INFO, "%s: Populating node First Element \n",__FUNCTION__);
3323             node->next = NULL;
3324             vsc8490_bounded_delay_interface_list.first = node;
3325         }else{
3326             syslog(LOG_INFO, "%s: Populating node at end of list \n",__FUNCTION__);
3327             /* Insert at the end of the list */
3328             run = vsc8490_bounded_delay_interface_list.first;
3329             while (run->next != NULL) {
3330                 run = run->next;
3331             }
3332             run->next = node;
3333             node->next = NULL;
3334         }
3335         vsc8490_bounded_delay_interface_count++;
3336         /* Display list for debug purpose */
3337         run = vsc8490_bounded_delay_interface_list.first;
3338         i = 1;
3339         syslog(LOG_DEBUG, " Displaying Elements in Linked list after add operation \n");
3340         while (run != NULL){
3341             syslog(LOG_DEBUG, " NODE no : %d\n",i);
3342             syslog(LOG_DEBUG, " run->secy_id.secy_vport_id : %d\n",run->secy_id.secy_vport_id);
3343             run = run->next;
3344             i++;
3345         }
3346         syslog(LOG_INFO,"%s: bounded delay interface count  : %d\n ",
3347                 __FUNCTION__,vsc8490_bounded_delay_interface_count);
3348 
3349      } else if ((macsec_settings->is_bounded_delay_enable) && !is_opAdd) {
3350         syslog(LOG_INFO, "%s: Operation Delete, to remove node  from list  is_opAdd:%d \n",
3351                __FUNCTION__, is_opAdd);
3352         /* Remove from data structure */
3353         if (vsc8490_bounded_delay_interface_list.first == NULL) {
3354             /* Empty list nothing to Remove */
3355             syslog(LOG_INFO,"%s:Empty List, Nothing to remove: boundedIntCount : %d\n ",
3356                    __FUNCTION__,vsc8490_bounded_delay_interface_count);
3357         }else{
3358             run = vsc8490_bounded_delay_interface_list.first;
3359             if ( (run->secy_id.secy_vport_id == secy_id->secy_vport_id) ){
3360                 vsc8490_bounded_delay_interface_list.first = run->next;
3361                 free(run);
3362                 vsc8490_bounded_delay_interface_count--;
3363             }else {
3364                 while ( (run != NULL) && (run->secy_id.secy_vport_id != secy_id->secy_vport_id) ){
3365                     prev = run;
3366                     run = run->next;
3367                 }
3368                 if (run == NULL ) {
3369                     syslog(LOG_INFO, "%s: Element not found in Linked list \n",__FUNCTION__);
3370                 }else{
3371                     prev->next = run->next;
3372                     free(run);
3373                     vsc8490_bounded_delay_interface_count--;
3374                 }
3375             }
3376             i = 1;
3377             run = vsc8490_bounded_delay_interface_list.first;
3378             syslog(LOG_DEBUG, "Displaying Elements in Linked list after Delete operation \n");
3379             while (run != NULL){
3380                 syslog(LOG_DEBUG, " NODE no : %d\n",i);
3381                 syslog(LOG_DEBUG, " run->secy_id.secy_vport_id : %d\n",run->secy_id.secy_vport_id);
3382                 run = run->next;
3383                 i++;
3384             }
3385             if (vsc8490_macsec_tx_lapn_thread_created && vsc8490_bounded_delay_interface_count == 0){
3386                  /* delete the thread */
3387                  thread = thread_get_by_name("vsc8490_macsec_tx_lapn_thread");
3388                  if (thread == NULL) {
3389                      syslog(LOG_ERR, "Failed to get the thread info.");
3390                      status = EFAIL;
3391                      return status;
3392                  }
3393                  /* Kill the thread */
3394                  thread_kill(thread);
3395                  syslog(LOG_DEBUG, " vsc8490_macsec_tx_lapn_thread KILLED suceesfully\n");
3396                  vsc8490_macsec_tx_lapn_thread_created = FALSE;
3397             }
3398         }/* End of delete from list */
3399 
3400     }else{
3401          syslog(LOG_DEBUG, "%s: Bounded delay not configured \n",__FUNCTION__);
3402     }
3403     syslog(LOG_INFO,"%s:bounded delay interface count  : %d \n",
3404            __FUNCTION__,vsc8490_bounded_delay_interface_count);
3405     if ( (vsc8490_macsec_tx_lapn_thread_created == FALSE) &&
3406           (macsec_settings->is_bounded_delay_enable) && is_opAdd ){
3407             /*
3408              * Start the background LAPN polling periodic thread
3409              */
3410         syslog(LOG_INFO,"%s:creating thread \n",__FUNCTION__);
3411         thread_create("vsc8490_macsec_tx_lapn_thread",
3412                       STACK_SIZE_LARGE,
3413                       THREAD_PRI_LOW | THREAD_NEEDS_TIMER_SERVICE,
3414                       vsc8490_macsec_tx_lapn_thread_fn,
3415                       NULL);
3416         vsc8490_macsec_tx_lapn_thread_created = TRUE;
3417     }
3418     return status;
3419 }
3420 /*
3421  * vsc8584
3422  */
3423 static void
vsc8584_macsec_tx_lapn_thread_fn(void)3424 vsc8584_macsec_tx_lapn_thread_fn (void)
3425 {
3426     wakeup_trigger_t                    trigger;
3427     void                                *trigger_context;
3428     timer_t                             tx_llpn_poll_timer;
3429     vsc8584_macsec_bounded_delay_node_t         *run;
3430     status_t                            status = EOK;
3431     ms_ciphersuite_t                    cipher;
3432     macsec_secy_id_t                    secy;
3433     ifd_t                               *ifd = NULL;
3434     ifl_t                               *ifl = NULL;
3435     ifd_macsec_t                        *macsec_settings = NULL;
3436 
3437     timer_init_leaf(&tx_llpn_poll_timer, NULL, 0);
3438     timer_start(&tx_llpn_poll_timer,VSC8584_MACSEC_TX_LAPN_POLL_TIMER_MS);
3439     vsc8584_bd_thread_start_time = time_get_ms();
3440 
3441     while (TRUE) {
3442         thread_sleep();
3443         trigger_context = wakeup_trigger_fetch(&trigger);
3444         switch (trigger.type) {
3445             case WAKEUP_TYPE_TIMER:
3446                 timer_start(&tx_llpn_poll_timer, VSC8584_MACSEC_TX_LAPN_POLL_TIMER_MS);
3447                     run = vsc8584_bounded_delay_interface_list.first;
3448                     while (run != NULL) {
3449                         ifd  = run->ifd;
3450                         ifl = run->ifl;
3451                         if (run->is_ifl_macsec && run->is_ifl_macsec != MACSEC_INVALID_VALUE) {
3452                             /* Polling IFL stats get */
3453                             if (ifl) {
3454                                 macsec_settings  = ifl->macsec_settings;
3455                                 cipher = ifd->macsec_settings->macsec_secy[0].ciphersuite;
3456                                 secy = run->secy_id;
3457                                 status = vsc8584_macsec_nextpn_stats_get(ifd,
3458                                                        secy.secy_vport_id,
3459                                                        secy.secy_service_id,
3460                                                        &macsec_settings->macsec_stats, cipher);
3461                                 if (status != EOK) {
3462                                     syslog(LOG_ERR,"%s:vsc8584_macsec_stats_get:status Error :%d \n",
3463                                            __FUNCTION__,status);
3464                                 }
3465                             } else {
3466                                 syslog(LOG_ERR,"%s:IFL is NUll \n",__FUNCTION__);
3467                             }
3468 
3469                         } else {
3470                             /* Polling IFD stats */
3471                             if (ifd) {
3472                                 macsec_settings  = ifd->macsec_settings;
3473                                 cipher = ifd->macsec_settings->macsec_secy[0].ciphersuite;
3474                                 secy = run->secy_id;
3475                                 status = vsc8584_macsec_nextpn_stats_get(ifd,
3476                                                  secy.secy_vport_id,
3477                                                  secy.secy_service_id,
3478                                                  &macsec_settings->macsec_stats, cipher);
3479                                 if (status != EOK) {
3480                                     syslog(LOG_ERR,"%s:vsc8584_macsec_stats_get:status Error :%d \n",
3481                                            __FUNCTION__,status);
3482                                 }
3483                             } else {
3484                                 syslog(LOG_ERR, "%s: NULL ifd \n",__FUNCTION__);
3485                              }
3486                         }
3487                         run = run->next;
3488                     }/* End of while loop */
3489             break;
3490             default:
3491                 wakeup_trigger_error(&trigger);
3492          }
3493     }
3494 }
3495 static status_t
gi2mic_vsc8584_macsec_tx_sa_lowest_pn_poll(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_secy_id_t * secy_id,boolean is_opAdd)3496 gi2mic_vsc8584_macsec_tx_sa_lowest_pn_poll (macsec_ifd_ifl_t *macsec_ifd_ifl,
3497                                           macsec_secy_id_t *secy_id,
3498                                           boolean is_opAdd)
3499 {
3500     status_t                            status = EOK;
3501     ifd_t                               *ifd = NULL;
3502     ifl_t                               *ifl = NULL;
3503     ifd_macsec_t                        *macsec_settings = NULL;
3504     vsc8584_macsec_bounded_delay_node_t         *node;
3505     vsc8584_macsec_bounded_delay_node_t         *run;
3506     vsc8584_macsec_bounded_delay_node_t         *prev;
3507     int                                 i = 0;
3508     thread_t                            *thread;
3509 
3510     MACSEC_GET_MACSEC_SETTINGS(macsec_ifd_ifl);
3511     if (macsec_ifd_ifl->is_ifl_macsec) {
3512         ifl = macsec_ifd_ifl->ifl;
3513         MACSEC_NULL_PTR_CHECK_RETURN_STATUS(ifl, "ifl is NULL");
3514     }else{
3515          syslog(LOG_INFO, "%s: ifd %d (%s) is_opAdd:%d \n",__FUNCTION__,
3516                 ifd->ifd_index, ifd->ifd_name, is_opAdd);
3517     }
3518     syslog(LOG_INFO,"%s: secy vport id: %d  is_bounded_delay_enable: %d \n",__FUNCTION__,
3519            secy_id->secy_vport_id, macsec_settings->is_bounded_delay_enable );
3520 
3521     if ( (vsc8584_bounded_delay_interface_count == VSC8584_MAX_SUPPORTED_BOUNDED_DELAY_INTERAFACE) &&
3522         is_opAdd ){
3523         syslog(LOG_ERR,"%s: Discarding as exceeding max supported:: %d \n",
3524                __FUNCTION__, vsc8584_bounded_delay_interface_count);
3525         return status;
3526     }
3527 
3528 
3529     if ( (macsec_settings->is_bounded_delay_enable) && is_opAdd ) {
3530         /* Add to data structure */
3531         node = (vsc8584_macsec_bounded_delay_node_t *)kzalloc(sizeof(vsc8584_macsec_bounded_delay_node_t));
3532         if (node == NULL) {
3533             syslog(LOG_ERR, "%s: Failed to allocate memory for macsec_bounded_delay_node \n",__FUNCTION__);
3534             status = EFAIL;
3535             return status;
3536         }
3537         node->secy_id.ifd_index       = secy_id->ifd_index;
3538         node->secy_id.secy_vport_id   = secy_id->secy_vport_id;
3539         node->secy_id.secy_service_id = secy_id->secy_service_id;
3540         node->secy_id.encryption      = secy_id->encryption;
3541         node->is_ifl_macsec           = macsec_ifd_ifl->is_ifl_macsec;
3542 
3543         if (macsec_ifd_ifl->is_ifl_macsec) {
3544             syslog(LOG_INFO, "%s: Populating node list ifl , vport id :%d  is_opAdd:%d \n",
3545                    __FUNCTION__, node->secy_id.secy_vport_id, is_opAdd);
3546             node->ifl                     = ifl;
3547             node->ifd                     = ifd;
3548         }else{
3549             syslog(LOG_INFO, "%s: Populating node list ifd %d (%s) is_opAdd:%d \n",
3550                    __FUNCTION__, ifd->ifd_index, ifd->ifd_name, is_opAdd);
3551             node->ifd                     = ifd;
3552             node->ifl                     = NULL;
3553         }
3554 
3555         if (vsc8584_bounded_delay_interface_list.first == NULL ){
3556             syslog(LOG_INFO, "%s: Populating node First Element \n",__FUNCTION__);
3557             node->next = NULL;
3558             vsc8584_bounded_delay_interface_list.first = node;
3559         }else{
3560             syslog(LOG_INFO, "%s: Populating node at end of list \n",__FUNCTION__);
3561             /* Insert at the end of the list */
3562             run = vsc8584_bounded_delay_interface_list.first;
3563             while (run->next != NULL) {
3564                 run = run->next;
3565             }
3566             run->next = node;
3567             node->next = NULL;
3568         }
3569         vsc8584_bounded_delay_interface_count++;
3570         /* Display list for debug purpose */
3571         run = vsc8584_bounded_delay_interface_list.first;
3572         i = 1;
3573         syslog(LOG_DEBUG, " Displaying Elements in Linked list after add operation \n");
3574         while (run != NULL){
3575             syslog(LOG_DEBUG, " NODE no : %d\n",i);
3576             syslog(LOG_DEBUG, " run->secy_id.secy_vport_id : %d\n",run->secy_id.secy_vport_id);
3577             run = run->next;
3578             i++;
3579         }
3580         syslog(LOG_INFO,"%s: bounded delay interface count  : %d\n ",
3581                 __FUNCTION__,vsc8584_bounded_delay_interface_count);
3582 
3583      } else if ((macsec_settings->is_bounded_delay_enable) && !is_opAdd) {
3584         syslog(LOG_INFO, "%s: Operation Delete, to remove node  from list  is_opAdd:%d \n",
3585                __FUNCTION__, is_opAdd);
3586         /* Remove from data structure */
3587         if (vsc8584_bounded_delay_interface_list.first == NULL) {
3588             /* Empty list nothing to Remove */
3589             syslog(LOG_INFO,"%s:Empty List, Nothing to remove: boundedIntCount : %d\n ",
3590                    __FUNCTION__,vsc8584_bounded_delay_interface_count);
3591         }else{
3592             run = vsc8584_bounded_delay_interface_list.first;
3593             if ( (run->secy_id.secy_vport_id == secy_id->secy_vport_id) ){
3594                 vsc8584_bounded_delay_interface_list.first = run->next;
3595                 free(run);
3596                 vsc8584_bounded_delay_interface_count--;
3597             }else {
3598                 while ( (run != NULL) && (run->secy_id.secy_vport_id != secy_id->secy_vport_id) ){
3599                     prev = run;
3600                     run = run->next;
3601                 }
3602                 if (run == NULL ) {
3603                     syslog(LOG_INFO, "%s: Element not found in Linked list \n",__FUNCTION__);
3604                 }else{
3605                     prev->next = run->next;
3606                     free(run);
3607                     vsc8584_bounded_delay_interface_count--;
3608                 }
3609             }
3610             i = 1;
3611             run = vsc8584_bounded_delay_interface_list.first;
3612             syslog(LOG_DEBUG, "Displaying Elements in Linked list after Delete operation \n");
3613             while (run != NULL){
3614                 syslog(LOG_DEBUG, " NODE no : %d\n",i);
3615                 syslog(LOG_DEBUG, " run->secy_id.secy_vport_id : %d\n",run->secy_id.secy_vport_id);
3616                 run = run->next;
3617                 i++;
3618             }
3619             if (vsc8584_macsec_tx_lapn_thread_created && vsc8584_bounded_delay_interface_count == 0){
3620                  /* delete the thread */
3621                  thread = thread_get_by_name("vsc8584_macsec_tx_lapn_thread");
3622                  if (thread == NULL) {
3623                      syslog(LOG_ERR, "Failed to get the thread info.");
3624                      status = EFAIL;
3625                      return status;
3626                  }
3627                  /* Kill the thread */
3628                  thread_kill(thread);
3629                  syslog(LOG_DEBUG, " vsc8584_macsec_tx_lapn_thread KILLED suceesfully\n");
3630                  vsc8584_macsec_tx_lapn_thread_created = FALSE;
3631             }
3632         }/* End of delete from list */
3633 
3634     }else{
3635          syslog(LOG_DEBUG, "%s: Bounded delay not configured \n",__FUNCTION__);
3636     }
3637     syslog(LOG_INFO,"%s:bounded delay interface count  : %d \n",
3638            __FUNCTION__,vsc8584_bounded_delay_interface_count);
3639     if ( (vsc8584_macsec_tx_lapn_thread_created == FALSE) &&
3640           (macsec_settings->is_bounded_delay_enable) && is_opAdd ){
3641             /*
3642              * Start the background LAPN polling periodic thread
3643              */
3644         syslog(LOG_INFO,"%s:creating thread \n",__FUNCTION__);
3645         thread_create("vsc8584_macsec_tx_lapn_thread",
3646                       STACK_SIZE_LARGE,
3647                       THREAD_PRI_LOW | THREAD_NEEDS_TIMER_SERVICE,
3648                       vsc8584_macsec_tx_lapn_thread_fn,
3649                       NULL);
3650         vsc8584_macsec_tx_lapn_thread_created = TRUE;
3651     }
3652     return status;
3653 }
3654 static status_t
gi2mic_macsec_tx_sa_lowest_pn_poll(macsec_ifd_ifl_t * macsec_ifd_ifl,macsec_secy_id_t * secy_id,boolean is_opAdd)3655 gi2mic_macsec_tx_sa_lowest_pn_poll (macsec_ifd_ifl_t *macsec_ifd_ifl,
3656                                     macsec_secy_id_t *secy_id,
3657                                     boolean is_opAdd)
3658 {
3659 
3660     if (GI2MIC_IS_VSC8490_PORT(macsec_ifd_ifl->ifd)) {
3661         return (gi2mic_vsc8490_macsec_tx_sa_lowest_pn_poll(macsec_ifd_ifl,
3662                                                       secy_id,
3663                                                       is_opAdd));
3664     } else {
3665         return (gi2mic_vsc8584_macsec_tx_sa_lowest_pn_poll(macsec_ifd_ifl,
3666                                                       secy_id,
3667                                                       is_opAdd));
3668     }
3669 }
3670 
3671 
3672 /*
3673  * gi2mic_macsec_config_vectors
3674  *
3675  * This function will initialize the MACsec specific vectors.
3676  */
3677 static status_t
gi2mic_macsec_config_vectors(pic_t * pic)3678 gi2mic_macsec_config_vectors (pic_t *pic)
3679 {
3680     if (!pic) {
3681         syslog(LOG_ERR, "%s: pic is NULL!\n", __func__);
3682         return EFAIL;
3683     }
3684 
3685     if (!pic->macsec_supported) {
3686         pic->macsec_supported = TRUE;
3687         pic->macsec_multi_secy_supported = TRUE;
3688 
3689         pic->macsec_capabilities = (MACSEC_CAPABILITY_CIPHER_SUITE_GCM_AES_128 |
3690                                     MACSEC_CAPABILITY_CIPHER_SUITE_GCM_AES_XPN_128 |
3691                                     MACSEC_CAPABILITY_CIPHER_SUITE_GCM_AES_256 |
3692                                     MACSEC_CAPABILITY_CIPHER_SUITE_GCM_AES_XPN_256);
3693         pic->phy_macsec_vector.initialization  = gi2mic_macsec_init;
3694         pic->phy_macsec_vector.tx_sc_create    = gi2mic_macsec_tx_sc_create;
3695         pic->phy_macsec_vector.rx_sc_create    = gi2mic_macsec_rx_sc_create;
3696         pic->phy_macsec_vector.sc_delete       = gi2mic_macsec_sc_delete;
3697         pic->phy_macsec_vector.sa_create       = gi2mic_macsec_sa_create;
3698         pic->phy_macsec_vector.tx_flow_create  = gi2mic_macsec_tx_flow_create;
3699         pic->phy_macsec_vector.rx_flow_create  = gi2mic_macsec_rx_flow_create;
3700         pic->phy_macsec_vector.flow_delete     = gi2mic_macsec_flow_delete;
3701         pic->phy_macsec_vector.macsec_block_enable  = gi2mic_macsec_block_enable;
3702         pic->phy_macsec_vector.macsec_block_disable = gi2mic_macsec_block_disable;
3703         pic->phy_macsec_vector.macsec_ifl_stats_get =
3704                                gi2mic_get_macsec_ifl_stats;
3705         pic->phy_macsec_vector.macsec_stats_get = gi2mic_get_macsec_stats;
3706         pic->phy_macsec_vector.macsec_port_reinit = gi2mic_macsec_port_reinit;
3707         pic->phy_macsec_vector.macsec_update_addr = gi2mic_macsec_update_addr;
3708         pic->phy_macsec_vector.macsec_periodic = gi2mic_macsec_event_handler;
3709         pic->phy_macsec_vector.macsec_init_post_issu = gi2mic_macsec_init_post_issu;
3710 
3711         pic->phy_macsec_vector.macsec_secy_create    = gi2mic_macsec_secy_create;
3712         pic->phy_macsec_vector.macsec_tx_sa_create   = gi2mic_macsec_tx_sa_create;
3713         pic->phy_macsec_vector.macsec_rx_sa_create   = gi2mic_macsec_rx_sa_create;
3714         pic->phy_macsec_vector.macsec_tx_activate    = gi2mic_macsec_tx_activate;
3715         pic->phy_macsec_vector.macsec_rx_activate    = gi2mic_macsec_rx_activate;
3716 
3717         pic->phy_macsec_vector.macsec_secy_delete    = gi2mic_macsec_secy_delete;
3718         pic->phy_macsec_vector.macsec_tx_sc_delete   = gi2mic_macsec_tx_sc_delete;
3719         pic->phy_macsec_vector.macsec_rx_sc_delete   = gi2mic_macsec_rx_sc_delete;
3720 
3721         pic->phy_macsec_vector.macsec_control_flow_create = gi2mic_macsec_control_flow_create;
3722         pic->phy_macsec_vector.macsec_control_flow_delete = gi2mic_macsec_control_flow_delete;
3723         pic->phy_macsec_vector.macsec_ifl_supported = gi2mic_macsec_ifl_supported;
3724         pic->phy_macsec_vector.macsec_rx_sa_lowest_pn_update = gi2mic_macsec_rx_sa_lowest_pn_update;
3725         pic->phy_macsec_vector.macsec_tx_sa_lowest_pn_poll = gi2mic_macsec_tx_sa_lowest_pn_poll;
3726 
3727     }
3728 
3729     return EOK;
3730 }
3731 
3732 static int
gi2mic_obj_create(ifd_t * ifd,u_int8_t phy_addr,lnk_mdio_vector_t * mdio_func,void ** obj)3733 gi2mic_obj_create(ifd_t *ifd,  u_int8_t phy_addr,
3734                   lnk_mdio_vector_t * mdio_func, void **obj)
3735 {
3736      status_t         err;
3737 
3738      if (!ifd) {
3739         return EFAIL;
3740      }
3741 
3742      phy_addr = gi2mic_get_phy_addr (ifd,phy_addr);
3743 
3744      if (GI2MIC_IS_VSC8490_PORT(ifd)) {
3745          vsc8490_create(ifd, phy_addr,mdio_func);
3746          VSC8490_GET_OBJ_FROM_IFD(ifd, *obj, err);
3747      } else {
3748          vsc8584_create(ifd, phy_addr,mdio_func);
3749          VSC8584_GET_OBJ_FROM_IFD(ifd, *obj, err);
3750      }
3751 
3752      return EOK;
3753 }
3754 
3755 void *
gi2mic_create(ifd_t * ifd,u_int8_t phy_addr,lnk_mdio_vector_t * mdio_func)3756 gi2mic_create (ifd_t *ifd, u_int8_t phy_addr, lnk_mdio_vector_t * mdio_func)
3757 {
3758     void *ptr = NULL;
3759     status_t        status;
3760     pic_t          *pic = NULL;
3761     mic_t          *dev;
3762     int            logical_port = 0;
3763 
3764     status = mic_link_from_ifd(ifd, &dev, &logical_port);
3765     pic = dev->pic_context;
3766 
3767     if (!pic) {
3768         syslog(LOG_ERR, "%s: Invalid PIC context\n", __FUNCTION__);
3769         return NULL;
3770     }
3771     gi2mic_macsec_config_vectors(pic);
3772 
3773     gi2mic_obj_create(ifd, phy_addr, mdio_func, &ptr);
3774 
3775     return ptr;
3776 }
3777 
3778 void
gi2mic_platform_init(mic_t * dev)3779 gi2mic_platform_init(mic_t *dev)
3780 {
3781     gi2mic_ge_mode[dev->pic_slot / MAX_PICS_PER_MIC] = 0xff;
3782     gi2mic_mdio_lock[dev->pic_slot / MAX_PICS_PER_MIC] = 0;
3783 
3784     GI2MIC_LOCK_CREATE(dev->pic_slot / MAX_PICS_PER_MIC);
3785 }
3786 
3787 /*
3788  *initialize all the ports
3789  */
3790 status_t
gi2mic_1g_link_init(pic_t * pic,ifd_t * ifd,int porttype)3791 gi2mic_1g_link_init(pic_t *pic, ifd_t *ifd, int porttype)
3792 {
3793     mic_t               *dev = pic->pic_context;
3794     status_t             err;
3795     int                  link;
3796     lnk_mac_ioctl_data_t mac_ioctl_data;
3797     lnk_phy_ioctl_data_t phy_ioctl_data;
3798 
3799     link                    = ifd->ifd_port - pic->pic_firstport;
3800 
3801 
3802     bzero(&mac_ioctl_data, sizeof(lnk_mac_ioctl_data_t));
3803     if (porttype != IFDP_XSERIES_10G) {
3804         mac_ioctl_data.cmd_type = LNK_MAC_IOCTL_SET_GE_MODE;
3805         mac_ioctl_data.lnk_mac_param.knob.onoff = FALSE;
3806        err = dev->lnk_mac_func->lnk_mac_ioctl(ifd, &mac_ioctl_data);
3807        if (err != EOK) {
3808           syslog(LOG_ERR, "%s: %s - Failed to set MAC layer ge mode \n",
3809                  __FUNCTION__, dev->pic_name);
3810           return EINVAL;
3811        }
3812     }
3813     /* Init external PHY */
3814     if (dev->mic_phy[link].lnk_phy_func) {
3815         err = (dev->mic_phy[link].lnk_phy_func)->lnk_phy_dev_init(ifd);
3816         if (err != EOK) {
3817             syslog(LOG_ERR, "%s: %s - Fail to init PHY %d\n",
3818                     __FUNCTION__, dev->pic_name, link);
3819             return err;
3820         }
3821     }
3822 
3823     if (((dev->mic_phy[link].lnk_phy_func) != NULL) &&
3824           ((dev->mic_phy[link].lnk_phy_func)->lnk_phy_ioctl != NULL)) {
3825           phy_ioctl_data.cmd_type = LNK_PHY_IOCTL_ACTIVATE_LINK;
3826           err = (dev->mic_phy[link].lnk_phy_func)->lnk_phy_ioctl(ifd,
3827                                                            &phy_ioctl_data);
3828           if (err != EOK) {
3829               syslog(LOG_ERR,"%s:%s -Fail to powerup external PHY, err(%d)\n",
3830                      __FUNCTION__, dev->pic_name, err);
3831              return err;
3832         }
3833     }
3834 
3835     if (dev->mic_sfp_phy[link].obj) {
3836             syslog(LOG_INFO, "%s: %s - powerup external SFP PHY\n",
3837                     __FUNCTION__, dev->pic_name);
3838             phy_ioctl_data.cmd_type = LNK_PHY_IOCTL_ACTIVATE_LINK;
3839             err = (dev->mic_sfp_phy[link].lnk_phy_func)->lnk_phy_ioctl(ifd, &phy_ioctl_data);
3840             if (err != EOK) {
3841                 syslog(LOG_ERR, "%s: %s - Fail to powerup external PHY, err(%d)\n",
3842                         __FUNCTION__, dev->pic_name, err);
3843                 return err;
3844             }
3845         }
3846 
3847     bzero(&phy_ioctl_data, sizeof(lnk_phy_ioctl_data_t));
3848     phy_ioctl_data.cmd_type = LNK_PHY_IOCTL_LINK_INIT;
3849     err = (dev->mic_phy[link].lnk_phy_func)->lnk_phy_ioctl(ifd, &phy_ioctl_data);
3850     if (err != EOK) {
3851             syslog(LOG_ERR, "%s: %s - Fail to init external PHY link\n",
3852                             __FUNCTION__, dev->pic_name);
3853             return err;
3854     }
3855     return EOK;
3856 }
3857 /*
3858  *Initialize all the ports
3859  */
3860 status_t
gi2mic_1g_link_init_all(mic_t * dev)3861 gi2mic_1g_link_init_all(mic_t *dev)
3862 {
3863     ifd_t       *ifd;
3864     pic_t       *pic;
3865     u_int16_t   porttype;
3866     u_int16_t   vsc_port = 0;
3867     status_t    err = EFAIL;;
3868 
3869     if (!dev) {
3870         return EFAIL;
3871     }
3872     for (vsc_port = 0; vsc_port < dev->port_num ; vsc_port++) {
3873         ifd = dev->ifd_list[vsc_port];
3874         pic = dev->pic_context;
3875         porttype = ifd->ifd_port_type;
3876         if (GI2MIC_IS_VSC8490_PORT(ifd)) {
3877         if (pic && ifd) {
3878             err = gi2mic_1g_link_init(pic, ifd, porttype);
3879             if (err != EOK) {
3880                 syslog(LOG_ERR, "%s: Failed to perform port init :%s\n",
3881                        __FUNCTION__, dev->pic_name);
3882                 return err;;
3883             }
3884         } else {
3885              syslog(LOG_ERR," %s: pic:%p OR ifd:%p for port:%d is NULL\n",
3886                         __FUNCTION__, pic, ifd, vsc_port);
3887              return EFAIL;
3888         }
3889         }
3890     }
3891     return EOK;
3892 }
3893 void
gmic2_service_thread_entry(void)3894 gmic2_service_thread_entry (void)
3895 {
3896     wakeup_trigger_t      trigger;
3897     void                 *trigger_context;
3898     timer_t               periodic_timer;
3899     mic_t                *dev;
3900 
3901     bzero(&periodic_timer, sizeof(timer_t));
3902     timer_init_leaf(&periodic_timer, NULL, 0);
3903     timer_start(&periodic_timer, GMIC2_BACKGROUND_PERIOD_MS);
3904     dev = thread_get_argument();
3905 
3906     while (TRUE) {
3907         thread_sleep();
3908 
3909         trigger_context = wakeup_trigger_fetch(&trigger);
3910         while (trigger_context) {
3911 
3912             switch(trigger.type) {
3913             case WAKEUP_TYPE_TIMER:
3914                 /*
3915                  *  Service thread wakes once per second, polls
3916                  *  status on every run -- the remaining are processed
3917                  *  at intervals set by these definitions.
3918                  */
3919                 gi2mic_1g_link_init_all(dev);
3920 
3921                 break;
3922 
3923             default:
3924                 wakeup_trigger_error(&trigger);
3925                 break;
3926             }
3927 
3928             /*
3929              * Look for the next wakeup trigger
3930              */
3931             trigger_context = wakeup_trigger_fetch(&trigger);
3932         }
3933     }
3934 }
3935 #endif /* GI2MIC_EXT_PHY_INIT */
3936
```