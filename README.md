## 焊盘制作方法
 - 打开 Padstack Editor
   - 贴片焊盘制作方法
     - File -> New ，选好名字和路径,Padstack usage选择 SMD，Select pad geometry中选择需要的形状,左下角Units选择Millimeter，因为很多手册都是用毫米标注的，所以这里也选择使用毫米
     - Design Layers ，Regular_Pad 中的 BEGIN_LAYER设置好焊盘相应尺寸,只设置Regular_Pad，其它不用，后面设计不要使用负片，就没问题，也不建议使用负片
     - Mask Layers
       - SOLDERMASK_TOP ，是阻焊层，尺寸要比焊盘大0.1MM，也可以是0.05MM
       - PASTEMASK_TOP , 是助焊层， 尺寸要和焊盘一样大
     - File -> Check ，检查是否有警告或者错误，无错误，点保存，就生成了pad文件
   - 通孔焊盘制作方法
     - File -> New ，选好名字和路径,Padstack usage选择 Thru Pin，Select pad geometry中选择需要的形状,左下角Units选择Millimeter，因为很多手册都是用毫米标注的，所以这里也选择使用毫米
     - Drill
       - Hole type 设置好通类型
       - Finished diameter 设置好钻孔的直径
       - Hole/slot plating ， 一定要选择Plated，铜壁上锡
     - Drill Symbol
       - Type of drill figure ，选择一个钻孔符号，我一般选择Hexagon x
       - Characters ， 输入一个符号，我一般填X
       - Drill figure width ， 填前面设置的钻孔直径
       - Drill figure height ， 填前面设置的钻孔直径
     - Design Layers
       - Regular_Pad 中的 BEGIN_LAYER设置好焊盘相应尺寸,右键复制，粘贴至DEFAULT_INTERNAL和END_LAYER层，这是设置每个层的焊盘，其它不用设置，不要使用负片，也不建议使用负片
     - Mask Layers
       - SOLDERMASK_TOP ，是顶层阻焊层，尺寸要比焊盘大0.1MM，也可以是0.05MM
       - SOLDERMASK_BOTTOM ，是底层阻焊层，尺寸要比焊盘大0.1MM，也可以是0.05MM
       - PASTEMASK_TOP , 是顶层助焊层， 尺寸要和焊盘一样大
       - PASTEMASK_BOTTOM , 是底层助焊层， 尺寸要和焊盘一样大
     - File -> Check ，检查是否有警告或者错误，无错误，点保存，就生成了pad文件
   - 槽孔焊盘的制作方法和通孔制作差不多，选择好类型就，其它一样
   - 过孔的制作方法和通孔也差不多，只是在Mask Layers中不要设置PASTEMASK_TOP就行，Design Layers中比孔径大0.1MM(4Mil),SOLDERMASK_TOP和SOLDERMASK_BOTTOM比Design Layers大0.1MM(4Mil)即可
   - 异形焊盘需要使用Flash，略微复杂，暂时不介绍了
## 元件封装制作的必要层
 - 打开 PCB Editor，新建一个Package symbol
 - Layout -> Pins -> 添加好每个PIN
 - 添加好相关PIN后，下面是要设置一些必要的层
   - Package Geometry
      - Silkscreen_Top　　//这是元件的丝印层
      - Silkscreen_Bottom　//如对底层有丝印需求，可以添加，但很少用到
      - Assembly_Top　　　//装配顶层，可以添加相关装配信息
      - Assembly_Bottom　//资本底层，如有需要也可以添加相关装配信息，但很少用到
      - Place_Bound_Top　//元件实际大小顶层，用于元件移动时，防止碰撞的，该层只能使用Shape制作
      - Place_Bound_Bottom　//元件实际大小底层，用于元件移动时，防止碰撞的，该层只能使用Shape制作,很少用到
  - 添加元件编号，Layout -> Labels -> RefDes
    - Ref Des
      - Silkscreen_Top //元件编号顶层,这是关系到元件在PCB上的丝印，大小一定要PCB厂家能够制作出来，具体要求可以看PCB厂家的文档，软件对应设置位置为:Setup -> Design Parameter -> Text -> Setup text sizes右边的三个点，Text Blk对应编号，Name对应别名，元件编号丝印就是选择的对应的别名
      - Assembly_Top　//元件编号的装配顶层,这可以选择更小的字体，只是方便布局时，用于查看编号的，和出装配图时，使用的
  - 添加元件Value，Layout -> Labels -> Value
    - Component Value
      - Silkscreen_Top　//元件值顶层,这是关系到元件在PCB上的丝印，大小一定要PCB厂家能够制作出来，具体要求可以看PCB厂家的文档，软件对应设置位置为:Setup -> Design Parameter -> Text -> Setup text sizes右边的三个点，Text Blk对应编号，Name对应别名，元件值丝印就是选择的对应的别名，但是这一层，很少会用，一般出丝印层的时候，都是出元件编号，很少出元件值，但也有用到的
      - Assembly_Top　//元件值的装配顶层,这可以选择更小的字体，只是方便布局时，用于查看编号的，和出装配图时，使用的
## Allegro PCB 前期设置
  - setup
    - Cross-section 设置板层可以添加多层，内电层一般设置为Plane，中间必须间隔Dielectric，也就是添加2层，就要添加4层,不要设置负片
    - Design Parameter
      - Display 中Enhanced display modes全部钩上
      - Design 中设置大小和单位，Mils时，小数位选择2位，因为光绘文件只能达到这个精度
      - Text 是设置字体大小这些的
    - Constraints
      - Modes
        - Electrical 中，Total etch length 和All differential pair checks 设置为On ，这是打开布线长度和差分布线时的相关显示
    - 相对延迟和差分布线，有些复杂，篇幅有限
## Allegro PCB Artwork 相关设置
  - 默认过孔开窗
  - 想要设置过孔盖油，需要将顶层和底层的阻焊层中的 过孔(Via)层删除，删除就表示过孔不开窗，也就是盖油的意思。
  - Manufacture
    - NC
      - NC Parameters 要把 Enhanced Excellon format 钩上，不然在有槽孔时，板厂软件可能无法识别
      - NC Route 把文件后缀修改为slot，不然在有槽孔时，板厂软件可能无法识别
## 尺寸标注方法
 - Manufacturer
  - Dimension Environment　//右键中Linear dimension 就是标注尺寸，属性option里面的Text添加%v%u，来标注尺寸，然后右键菜单中有很多关于尺寸相关功能
## 动态铺铜，可以在Shape -> Select Shape or Void/Cavity 选择铜皮后，右键，选择Parameters -> Thermal relief connects 中设置铺铜规则
## 板子外形
  - Board Geometry
    - Design_Outline中画板子外形,必须使用闭合的去画，用line不能画，然后再设置倒角之类的
## 禁止布线层
  - Setup
    - Areas
      - Route Keepin ，可以使用菜单Edit Z-Cpoy来进行操作，内缩0.2MM左右，不要和边框一样大
      - Package Keepin ， 可以使用菜单Edit Z-Cpoy来进行操作，可以和边框一样大