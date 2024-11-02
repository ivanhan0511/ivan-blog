---
title: "C++"
date: 2024-11-02T10:46:00+08:00
categories:
- C/C++
tags:
- C/C++
keywords:
- c
- c++
clearReading: true
#thumbnailImage: //example.com/static/A.png
thumbnailImage: image-1.png
thumbnailImagePosition: bottom
autoThumbnailImage: yes
metaAlignment: center
#coverImage: //example.com/static/B.png
coverImage: image-2.png
coverCaption: "A beautiful image"
coverMeta: out
coverSize: full
comments: false
showTags: true
showPagination: true
showSocial: true
showDate: true
---

This is a custom summary and does *NOT* appear in the post.
<!--more-->

{{< toc >}}

## I. IDE
---
ͬʱ�� vim ��CLion ��vscode ���� C++����ͦ��һ��ʱ�䣬����һ���Լ��ľ��顣
vim����������˳�ֵģ�����һ��ѧϰ���߶��͵����⡣vim �����˵��Ͷ���������ߵ��ˣ�һ��ѧϰ���������档VIM �Ը��� C++��Ŀ����Ӧ������õģ�ʹ�� YouCompleteMe ��� clangd �������� cmake ��Ŀ���� makefile ��Ŀ��ͳͳ���� compile_commands.json ֮��Ϳ����޷��ν��ˡ� ͬʱ����� gtags-cscope ֮�󣬻����� Find References Ҳ�Ƚ�˿���ˡ� ���� easymotion ��multicursor ���� vim �ϱر����ˡ����Կ��� skywind3000 �������ʹ�� vim � C++���������ģ���Ӧ����Ҫ�� Windows �����Ƚ϶࣬�� Mac ��Ҳ���Ƶģ���Ҫ�������ԣ��������� ubuntu �� vm �Ƚ�����һЩ��

vscode��clangd + microsoft cpp �����������Կ� clangd �ٷ���վ�Ľ��ܵ� best pracitce( https://clangd.llvm.org/installation)��������������ı༭��������ö�����ϸ���ܡ� vscode ��װ vim ���֮���Դ��� easymotion ��һЩ�ȽϺ��õ� vim ����������˵�� JetBrains �ҵ� IdeaVim Ҫ����ȫ��һЩ������ʵ���������ƺ�û�� IdeaVim ��ô�ȶ���vscode �ĺô��ǿɶ����Էǳ�ǿ���� vim һ������ cmake �� makefile ��Ŀ���ܱȽϺõ�֧�֣�������ʾ����̬��������Щ���� clangd �ɵ���Ҳ���ܸɵúá� ͨ����������֮��vscode Ҳ��������������ȫ�ü��̲����ˡ� �������ͨ���Ļ���vscode �Ĺ���������Ҫ����һ�£���Ȼ�� mac ���������ǳ����ʣ��� trackpad �� magic mouse �Ļ��Ǹо����������������ġ�
vscode ��������Ƹ�����Ϊ�� remote ģʽȷʵ���÷ǳ������� Clion Ҫ�úܶ�ܶ�ܶࡣ���������� ���� vscode ʱ��ʱ����һЩС���⣬���糷���༭���������cmd+z �� vim ��� u ���о��г�ͻ�����ǲ�С�ľͳ������ˡ�

clion�� ����ʵҲ�ǻ��� clangd ����ģ������˵���ӿ��伴�õö࣬������ 0 ���þͿ����ˣ�����༭������ܱ������ĸ�����Ϊ�� vscode Ҫ����˿���ܶ࣬����ĳ���ȱ� vscode ��һЩ�����������ٺܶ࣬������һЩ���ܾͲ���ʵ�֡�����Ҳ��һЩ���� multi-highlight ���ֺ��õĲ����vscode û�ж�Ӧ�ġ�clion �Ĵ����������û�ȽϺ���һЩ���� idea �Ǹ�������һƴ�ˣ������Ŀ����ȫ�� cmake ����û�� extenral_project �Ļ�����ô clion �������Ƿǳ����ġ�
clion ��ȱ�������Ҳ��Ҫ���������� remote development ������һ���Ѿ����� vscode �� remote ��ȼ�ֱ�� beta ���㲻�ϣ���ʹ�ѷ������� clion ���ڴ�Ѵ�С���� 12G ���ϣ�Ҳûʲôʵ�ʵ���������΢��һ�����Ŀ��������������ʧЧ�ˣ�Ȼ�󿨰��콨�������������һ���Ѿ���
clion ����һ��ȱ�����ֻ�ܶ� cmake �������Ŀ�бȽϺõ����飬һ�������Ŀ����� cmake �� makefile ������ cmake ������ external_project ���Ǵ��������������ͷǳ�׽���ˡ�
���˺ܾ� clion ����Щ���⣬������ʹ���ˡ� ��Щ C++��Ŀ�� Mac �ϱȽ��Ѹ㣬���� remote �Ǹ��衣


������˵��vim ��һ����ѧϰ���ߣ���ҪͶ��һЩʱ�䣬Ч�������ط���û����ô�졣���ǻ��ǽ���һ��Ҫѧϰ��ʹ�� vim ����Ϊ��ʹ vscode ��clion ��Щ���ڰ�װ vim ���֮����и��ߵ�Ч�ʡ� �������� vim ����Ĺ����У����ܹ��� c++���������еĺܶ�ϸ���и��õ���⣬���� clangd �Ĵ�����ʾ��ô�������ɣ�clang-tidy ����ô���£�clang-format ������ô���£��ֱ�����ô���õģ�vim �� git ��ô���ɵģ�git �ڲ������ݽṹ�����ʲô���ģ��ȵȡ�
������ʹ�� vscode ��clion ��ʱ�򣬶��ںܶ�����ͻ��и���͸������⣬��������ȫֻ�ܵ���һ���ں�ʹ�á�

�����������ʹ�õ��� vscode �� 70%ʱ�䣩������ʹ�� vim �� 25%ʱ�䣩������ʹ�� clion �� 5%ʱ�䣩��vscode ��ͬʱʹ�� clangd ��clang-format ��Ϊ�˸�ʽ�� proto ��clangd �������û�� format proto �ļ�����microsoft cpp ��vim �ȵȲ����ƽʱ��Ҫ���� vscode �� remote ģʽ������ͻ������ŷ������ϣ�ubuntu ��centos ��rocky ���У���������������Docker Container ���У�������˵�����ǱȽϺõġ�
vim Ч�ʸ� vscode ��࣬���� vscode ż����ЩСë����vim �������ܻ�ҪЧ�ʸ��ߣ����� vim ����Ҫ������ YouCompeteMe ��������һЩ�ϵ�ϵͳ�ϱ��������Ƚ��鷳��ÿ��������ȥ���û���Ҳ�Ǹ�ͦ�˷�ʱ����£�vscode ��ʡ�ĺܶ࣬�Զ���װ�����ˡ�

��������˵�˺ܶ࣬ϣ���� OP ���а�������


