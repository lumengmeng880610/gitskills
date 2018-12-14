git 的创建和使用总结
1 在git里创建一个版本库，git是分布式的版本库。在git里依次输入：
 mkdir learngit    //创建一个git版本库；   //使用window系统，为了避免各种各样的问题，请确保目录名不包含中文。
 cd learngit   //转到learngit文件夹；
 pwd    //显示当前目录  ；
2  通过git init命令把这个目录变成git可以管理的仓库；   git init
//把文件添加到版本库
     所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件，网页，所有的程序代码等等，git也不例外。版本控制系统可以告诉你每次的改动。
     而图片，视频这些二进制文件，虽然也能由版本控制系统管理，但没法跟踪文件的变化，只能把二进制文件每次改动串起来，即只知道图片从100k改成120k，但是到底改的什么内容，版本控制系统不知道，也没法知道。
	microsoft的word格式是二进制格式，因此，版本控制系统时没法跟踪word文件的改动的，前面我们举的例子只是为了演示，如果要真正使用版本控制系统，就要以纯文本方式编写文件。
	因为文本是有编码的，比如中文有常用的GBK编码，如果没有历史遗留问题，建议使用标准的UTF-8编码，所有语言使用同一种编码，既没有冲突又被所有平台所支持。
	千万不能用windows自带的记事本编辑任何文本文件。因为microsoft开发记事本的团队使用了一个非常弱智的行为来保存utf-8编码的文件，它们在每个文件开头添加了
 	0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个'？'，明明正确的程序一编译就报语法错误等等，都是记事本的行为带来的。
	建议你下载Notepad++代替记事本，不但功能强大，而且免费。记得把Notepad++的默认编码设置为UTF-8 without BOM即可：
3 编写一个txt文件
	在learngit目录下添加文件到仓库：   git add readme.txt      //执行命令没有任何显示，就说明成功了。注：一定要现在文件夹里新建一个readme.txt	
	然后用命令git commit 告诉git，把文件提交到仓库：git commit -m "wrote a readme file";   //git commit 命令-m后输入的是本次提交的说明，可以输入本次的改动记录。
	git commit 命令执行成功后会告诉你， 1： file changed ： 1个文件被改动；2：insertions：加入两行内容。
	//git添加文件需要add, commit 2步，因为commit可以一次提交很多文件，所以你可以多次add不同的文件；
	git add file1.txt    git add file2.txt file3.txt git commit -m "add 3 files."
	冲突的处理： git status  查看冲突问题；
	git diff git1.txt          //git diff 就是修改内容之后的文件；
二： 时光机穿梭
	每次修改了内容之后，都要更新文件的版本。
	git add git1.txt     //新增更新的文件
	git commit -m "append GPL"    //提交更新内容
	git log   //查看更新的内容的每个版本记录，每次更新都有一个commit id ；
	git log --pretty=oneline    //输出的是每次更新的commit id ，这个比较简洁，只有id 和 注释内容；
	// 如果想把文件回退到上一个版本也是可以的。我们可以启动时光穿梭机，准备把git1.txt回退到上一个版本；git必须知道当前版本是哪个版本，在git中，
	用HEAD表示当前版本，也就是最新的提交；上一个版本就是HEAD^，上上一个版本就是HEAD^^, 往上几个版本就是HEAD加几个^，所以写成HEAD~100。
	git reset --hard HEAD^        //回退到上一个版本
	git reset --hard 版本号          //返回到已经回退的那个版本，版本号可以不用全写，写前几位就可以了；
	cat git1.txt    //显示这个文件的当前的内容；
	在git中总是有后悔药的，如果文件很多，想找到要恢复的id不容易，所以git提供了一个命令git reflog 用来记录你的每次命令：
	git reflog   //每条记录的id和每条记录的内容注释；
	
//git 和其他版本控制系统如svn的一个不同之处就是有暂存区的概念；
	工作区（working directory）: 就是电脑里能看到的目录，比如我的learngit文件夹就是一个工作区；
	版本库（repository）: 工作区有一个目录 .git，这个不算工作区，而是git的版本库；
	git的版本库里存了很多内容，其中最重要的就是称为stage（或叫index）的暂存区，还有git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD;
	工作区--> add --> 版本库（stage-->commit --> master）
	// 我们把文件往git版本库提交时，分两步执行：
	    1：用git add 把文件添加，实际上就是把文件修改添加到暂存区；2：用git commit提交更改，实际上就是把暂存区的所有内容提交到暂存区；
	//因为我们创建git版本库时，git自动为我们创建了唯一一个master分支，所以，现在git commit 就是往master分支上提交更改。
	需要提交的文件修改都放到暂存区，然后一次性提交暂存区的所有修改。
	git add *    //添加所有的文件到git仓库暂存区，* 表示所有改动的和新增的文件；
	git commit -m "understand how stage works"   //提交到git仓库
//管理修改
	为什么git比其他版本控制系统设计的优秀，因为git跟踪并管理的是修改，而非文件。
	修改就是：你新增，删除，修改的信息都算修改；新建一个新文件也算是修改。
	比如一个文件的修改：第一回修改--> git add -> 第二回修改 --> git commit    //结果是只提交了第一次修改的内容，没有提交第二次修改的内容；因为git管理的是修改，当你用git add命令后，在工作区的第一次修改就被放入暂存区，准备提交，但是第二回修改并没有放入暂存区。所以git只负责把暂存区的修改提交了，所以只提交第一个修改。
	//每次修改，如果不用git add到暂存区，就不会被加入到commit。
//撤销修改
	当你修改了内容之后，发现修改错了，其实除了手动修改文档之外，还可以用另一个语法撤销修改；
	git checkout -- git1.txt    //撤销修改   git checkout -- 文件名
//删除文件
	rm git3.txt    //例如删除这个文件，
////  远程仓库
   //添加远程库
	如果你已经在本地创建了一个git仓库后，又想在github创建一个git仓库，并且让这两个仓库进行远程同步，这样，GitHub的仓库既可以作为备份，又可以让其他仓库来协作。
	1：登录GitHub，在右上角点击加号，找到 new repository 	创建一个新的仓库；
	2：在 repository name 填上仓库名字, 比如 learngit , 其他可以不填保持默认，然后点击 create repository ，就成功创建一个新的git 仓库；
	3：现在GitHub上的learngit仓库还是空的，GitHub可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后把本地仓库的内容推送到GitHub仓库。
	4：可以根据GitHub的提示，在本地的learngit仓库下运行命令： 
		git remote add origin https://github.com/lumengmeng880610/learngit.git
	5：添加后，远程库的名字就是origin，这个是git的默认叫法，也可以改成别的，但是origin一看就知道是远程库。
		git push -u origin master    //用git push 命令，实际上就是把当前分支master推送到远程。由于远程库是空的，我们第一次推送master时，加上了-u参数，
	Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
	以后只要本地有提交，就可以通过命令： 
	git push origin master     //把本地master分支的最新修改推送到GitHub，就拥有了真正的分布式版本库了。
	// SSH警告
	第一回使用git的clone或push命令连接GitHub时，会得到一个警告：这是因为git使用SSH连接，而SSH连接在第一回验证GitHub服务器的key时，需要你确认GitHub的key的指纹	信息是否真的来自GitHub的服务器，输入yes即可。
	git会输出一个警告，告诉你已经把GitHub的key添加到本机的一个信任列表里：这个警告只会出现一次，后面的操作就不会有任何警告了。
	如果你实在担心有人冒充GitHub服务器，输入yes前可以对照GitHub的RSA Key的指纹信息是否与SSH连接给出的一致。
   // 从远程库克隆
	如果从最开始先创建远程库，然后从远程库克隆。首先登录GitHub，创建一个新的仓库  gitskills；
	我们勾选上 intialize this repository with a READEME, 这样GitHub就会自动为我们创建一个READEME.md文件，创建完之后可以看到READEME文件；
	然后就可以克隆一个到本地库： 
		git clone https://github.com/lumengmeng880610/gitskills  
	现在到目录下的git bash命令行里输入 :
		ls    //会出现READEME.md  文件
	如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。
	//使用https除了速度慢以外，还有个最大的麻烦就是每次推送都必须输入口令，但是某些只开放http端口的公司内部就无法使用ssh协议而只能使用https。
	//Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。	
////分支管理
	git的分支无论创建，切换和删除分支，git在1秒之内就能完成，无论你的版本库是1个文件还是1万个文件。
   //创建与合并分支
	在版本回退里，每次提交git都把它们串成一条时间线，这个时间线就是一个分支。截止到目前，只有一条时间线，在git里，这个分支叫主分支，即master分支；
	HEAD严格来说不是指向提交，而是指向master，master才是指向提交的，所以，HEAD指向的就是当前分支。
	一开始的时候，master分支是一条线，git用master指向最新的提交，再用HEAD指向master，就能确定当前分支，以及当前分支的提交；
	每次提交，master分支都会向前移动一步，这样，随着你不断提交，master分支的线也越来越长；
	当我们创建新的分支，比如dev，git新建一个指针叫dev，指向master相同的提交，再把HEAD指向dev，就表示当前分支在dev上；
	git创建一个分支很快，因为除了增加一个dev指针，改改HEAD的指向，工作区的文件都没有任何变化。
	不过，从现在开始，对工作区的修改和提交就是针对dev分支了，比如新提交一回，dev指针往前移动一步，而master指针不变；
	如果，我们在dev上的工作完成了，就可以把dev合并到master上。git怎么合并呢？最简单的方法，就是直接把master指向dev的当前提交，就完成了合并。
	所以git合并分支就改改指针，工作区内容也不变；
	合并完分之后，甚至可以删除dev分支，删除dev分支就是把dev指针给删掉，删掉后，我们就剩下了一条master分支；
	1：git checkout -b dev               // 创建分支dev，然后切换到dev分支；-b表示创建并切换到分支；
	2：git branch dev   // 创建分支dev；    git checkout dev    //切换到分支dev；
	3：git branch   // 查看当前所有分支；当前分支前会标有一个 * 星号；
	4：我们可以在dev分支上正常提交，比如修改一个文件 test.txt ，然后提交，git add test.txt   git commit -m "branch test"
	5：git checkout master   // dev的工作完成，切换到主分支master；查看一下，刚刚修改的内容没有出现，因为刚刚在dev分支提交的内容，master没有提交，所以没有修改；
	6：git merge dev                 //  把dev分支的工作成果合并到master分支上；
	// git merge 命令用于合并指定分支到当前分支，合并后，再查看test.txt的内容，就可以看到，和dev分支的最新提交是完全一样的。
	// 会出现 Fast-forward 信息，这个合并是快进模式，也就是把master指向dev的当前提交，所以合并速度很快；但并不是每次合并都Fast-forward，还会有其他的合并；
	7： git branch -d dev       // 合并完成以后就可以放心的删除dev分支了；然后在查看git branch, 就只有master分支了；
	// 因为创建、合并和删除分支非常快，所以Git鼓励你使用分支完成某个任务，合并后再删掉分支，这和直接在master分支上工作效果是一样的，但过程更安全。
   //解决冲突
	//合并分支往往不是一帆风顺的；所以要学会解决冲突；
	1： git checkout -b feature1    //创建一个新的分支feature1
  	2：修改一下test.txt 文件；
	3：git add test.txt   git commit -m "and simple"    //在feature1分支上提交修改；
	4：git checkout master     //切换到主分支master；再次修改test.txt文件，修改内容和featrue1分支的内容不一样；
	5：git add test.txt   git commit -m "and simple" 现在master分支和feature1分支各自都分别有新的提交；
	6：冲突了，test.txt文件存在冲突，必须手动解决冲突后再提交。git status也可以告诉我们冲突的文件；（如果feature1分支的修改和master的修改一样，就不会报错）
	7：git add test.txt      git commit -m "conflict update"    //修改冲突后提交；
	8：git log --graph --pretty=oneline --abbrev-commit   // 用带参数的git log也可以看到分支的合并情况：
	9：git branch -d feature1       //删除feature1分支
	总结：
	当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
	解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
	用git log --graph命令可以看到分支合并图。
   //分支管理策略
	// 通常，合并分支时，git会用Fast forward 模式，但这种模式下，删除分支后，会丢掉分支信息；
	如果自己强制禁用Fast forward 模式，git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
	1：git checkout -b dev   //新建一个分支；然后修改test.txt的内容；
	2：git add readme.txt    git commit -m "add merge"            // 提交修改；
	3：git checkout master    //切换回master分支；
	4：git merge --no-ff -m "merge with no-ff" dev           //  合并dev分支， --no-ff 表示禁用 Fast forward 模式；
	5：git  log --graph --petty=oneline --abbrev-commit      //查看分支情况 ； 要想退出这个模式，可以按英文状态下的 q 键；
	//分支策略
	在实际开发中，应该按照几个基本原则进行分支管理：
	首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在master上写东西；
	最好在分支上写内容，比如dev分支，也就是说dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，
	在master发布1.0版本；团队的每个小伙伴都在dev分支上写东西，每个人都有自己的分支，时不时地往dev分支上合并就可以的。
  	
