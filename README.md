# proj106-yaml-python-tool-for-OS
多场景OS的YAML+python定制工具

## 项目描述

linux kernel可以统一支持从服务器到嵌入式，云、边、端等多种场景。然而linux OS生态一直处于碎片化状态，没有一个OS能够同时覆盖各大场景，并方便的支持场景化定制构建。比如yocto支持基于overlay的灵活定制，然而主要面向嵌入式场景，并且与构建系统耦合，复杂度高。而服务器、桌面上基于RPM/DEB的主流linux OS则定制能力有限。要打造一个统一OS，就要在统一基础上实现场景化定制。

在统一OS中，各软件包用YAML描述，各场景的定制选项也用YAML描述。YAML内支持python定制逻辑，用于描述各字段之间的逻辑关系。在此基础上，需要设计实现一个强大而灵活的工具，把各YAML文件合并、求值，完成OS选项定制。基于这一功能，可以形成类似yocto overlay的定制能力，但是设计更为通用化，不但可以应用于统一OS，也可以应用在其他需要灵活定制能力的项目上。

## 所属赛道

2025全国大学生操作系统比赛的“OS功能设计”赛道

## 参赛要求

- 以小组为单位参赛，最多三人一个小组，且小组成员是来自同一所高校的本科生
- 如学生参加了多个项目，参赛学生选择一个自己参加的项目参与评奖
- 请遵循“2025全国大学生操作系统比赛”的章程和技术方案要求

## 难度

大（可提供额外指导和参考材料）

## 特征

OS定制体现为，含代码逻辑的大量YAML文件及python函数库，
按一定策略进行数据合并、变换、求值，输出为一组数据JSON，映射为具体包格式。

流程：

```
	input project/layer/package yaml files (with embedded python code)
	    + library python files
	=> composite
	=> per-package-file aggregate/merge
	   => merge_function1 # merge 2 layers for a field
	   => merge_function2
	   => merge_function3
	=> data pipeline 
	   => action_function1 # handle 'key:append': val
	   => action_function2
	   => option_function1 # handle option.openssl: true
	   => option_function2
	=> embedded python code delayed evaluation
	=> sanity checks
	   => sanity_function1
	   => sanity_function2
	=>
	output package files (YAML)
	with extra info (for debug, cache, etc.)
```

样例:

baseos/package1.yaml
		key1: val1
		key2: {{ python code on self.key1 }}

```
	layer1/package1.yaml
		key1: val2
	
	layer2/package1.yaml
		key2: {{ updated python code on self.key1 }}
	
	after merging baseos+layer1+layer2
		key1: val2
		key2: {{ updated python code on self.key1 }}

	# python code表述了字段之间的逻辑关系
	# 实际求值过程应当惰性、递归进行
	# key1 finalize后，对 key2 求值，得到确定性结果
```

## 进阶特性

- 易理解、易定制、易调试、不易出错、多而不乱、可以scale
- 通用化，架构设计上支持换语言

## License

MulanPSL-2.0+

## 预期目标

1. 一个通用YAML定制工具，适用于任意项目，作为传统YAML模板定制方式之外的另一种选择
2. 一个扩展工具，支持OS定制的额外需求


### 项目导师

吴峰光

* gitee [Fengguang](https://gitee.com/wu_fengguang)
* email wfg@mail.ustc.edu.cn
