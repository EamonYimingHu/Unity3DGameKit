# Unity3DGameKit

在对3DGameKit音频重置的过程中，熟悉游戏音频的工作流，思考和了解不同的音频系统的实现方法

● 独立完成了音效设计与制作（从初期采样到DAW中的音效制作与处理），借此了解音效师的工作流

● 了解其游戏机制与脚本实现，提升对游戏代码的理解能力，方便后续通过编写，修改C#脚本实现音效触发；同时了解原本的Unity音频系统实现方法

● 禁用其本身的Unity音频系统后，思考不同的音效触发的实现方式

游戏片段视频展示：
Youtube链接：https://youtu.be/n4FCFsN1X0U
Onedrive链接：https://1drv.ms/v/s!Av502t1TJ38dgTgvRrTRbIJDWvvD

以下仅举1-2例说明

# 攻击ATTACK音效的不同实现方法
## 简述声音设计部分

主角有四连击的Combo，并且需要分开攻击(Attack)音效与受击音效（Hit），同时考虑击中时需要提供正反馈音效（Impact）。因此先设计一个多层音效叠加出的Attack，再通过调整每层音效的具体Pitch，Level来做出Combo之间的区别。在此基础之上，为给玩家提供更强的正反馈，Impact和Hit需再单独设计，并整体混音时在听感上要占足够地位。

## 思考不同实现方法

● 利用游戏demo中已有的攻击系统，修改脚本，结合Wwise和脚本中的攻击逻辑实现Attack音效。

游戏本身具有战斗系统，考虑为共享同样的战斗机制的不同gameobject通过setOwner和if判断owner来分别实现不同的attack SFX
在BeginAttack方法下，通过判断攻击者类型（主角/怪物/BOSS），并触发相应的Ak.SoundEngine.PostEvent
示例代码片段：
![image](https://github.com/user-attachments/assets/2838416b-3d86-4456-86a3-4e818b5fc2d2)

● 利用动画系统Animator，Animation.

通过在动画关键帧绑定Animation EVENT，可通过在脚本中添加public方法，为这类通过动画关键帧触发音效的统一设置postevent来触发音效

![image](https://github.com/user-attachments/assets/7ba9304c-157d-4142-a362-9572a6ba9bc7)

● Unity音频系统方式触发：

通过攻击系统中BeginAttack下判断if (attackAudio != null)，触发attackAudio.PlayRandomClip();

# 脚步声的切换


