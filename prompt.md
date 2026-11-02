# 我需要你帮我写一个skill 用来审核用户调用的微基因微解读代码。
该skill需要读取用户的代码和配置文件（一个.zip文件）和用户提供的文献连接或者标题。主要审核点是：
1. 检查用户是否对no_call位点做特殊处理和适配。（如果是prs risk计算的应用，通常应该由人群频率值来取个平均值代替）。
2. 如果是prs risk应用，需要检查用户使用的位点effect allele，beta/or值是否和文献值一致。（需要联网读取paper内容，有可能需要读取附件，gwas summary一般在附件里）
3. 是否用对or和 beta的计算（or要累乘，beta要累加）,人群平均值的算法是否正确。（2*beta*maf或者 math.pow(_or, 2*maf)）
4. 用户提取genotype时，是否对rsid进行大写转换， 因为wegene的 input里rsid是大写的。
5. 输出每项审核结果。
6. 所有项都通过，则输出用户应用通过审核。
7. wegene微解读的开发文档连接：https://github.com/wegene-llc/weapp-developer-guide 你可以读下。
8. 一个比较好的prs应用示例：https://github.com/TuBieJun/wegene_weijiedu_example/

追加需求：
1. 需要调整下skill, 我们不检查方法是否足够严谨
2. "少量离散位点评分转换为未经验证的正态百分位"这种我们不管.
3. “但 L222–249 在无法计算时仍通过 stdout 返回正常结果，异常输入也缺少友好错误处理”这种我们也不管。
4. 我们只检查明显的错误,effect allele/beta/or 与文献不一致这种，以及no call位点的处理。
5. 这个skill需要开始的时候有个交互的选项，提示使用者输入代码.zip的文件路径和paper文献标题或者连接
6. 这个skill需要弄成git项目，关联到远程仓库https://github.com/TuBieJun/wegene-micro-app-audit,进行版本管理，并增加readme.md。
