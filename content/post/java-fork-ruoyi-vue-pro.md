---
title: "How to fork ruoyi-vue-pro"
date: 2024-09-29T14:43:47+08:00
categories:
- Java
- WebFramework
tags:
- Java
- Springboot
- ruoyi-vue-pro
keywords:
- Java
- spingboot
- ruoyi-vue-pro
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

To deploy a Springboot web framework, whatever development or production, and whatever single or distributed entity, 
[ruoyi-vue-pro](https://github.com/YunaiV/ruoyi-vue-pro) is a good project

But the stronger, the more complex. This post is the annotation for this project, by my personer opinion

<!--more-->

{{< toc >}}

The most important TECH cores are design and architecture which service for business, and then the second are IoC and AoP.

NO BEST, ONLY BETTER.


## Ŀ¼

- [I. ARCHITECTURE PRINCIPLE](#chapter-1)
- [II. LIFE CYCLE](#chapter-2)
- [III. IMPORTANT SUPPORT](#chapter-3)
- [IV. BUSINESS DESIGN](#chapter-4)
- [V. OPERATION](#chapter-5)
- [VI. APPENDIX](#chapter-6)




## I. ARCHITECTURE PRINCIPLE {#chapter-1}
---
So far, Sep 24, 2024
1. Reading the [docs](https://github.com/YunaiV/ruoyi-vue-pro) of ruoyi-vue-pro is the best way

2. ����ŶӱȽ�С, �ﲻ���ϰ���, ��ô�Ͳ������ֱ�ӵ�������, ������Ƶ�"��", �ǳ���Ҫ!!!  
   �����ж�̬��������ͬ��ģ���ɲ�ͬ���Ŷӵ���ά��������ģ�鵥����֮������󣬷�������״�����

3. ���ֶ���Ŀ���󳧺ܶ���ĿҲ�������ַ�ʽ. ��Щ��˵�ᷢչΪ�ֲ�ʽ����ģ��Ƕ�maven���������ɡ���ά�Ƕ����������ֺͶ��git����Ĵ�����ﶼһ���ģ�ֻ�ǹ���ʽ��һ��

   ��Щ"α΢����"��Ŀ�����������͵�cloneһ�Ѳֿ⣬���������������ĳ���ֿ�����˶�����ģ��Ĳֿ�û���¡���

3. ����, git�Դ���ģ��, ��һ���ܵĲֿ�, ����������Ϊ��ģ��汾���Թ���, Ҳ��һ�ּ��ݷ���

5. ���ܴ�������DO, ����������DDD(�����������)!!!




## II. LIFE CYCLE {#chapter-2}
---
���ۻ���SpringBoot�Ŀ������ô�仯, �˽�Spring����������, �Լ�����IoC�����������,�����ص�

�˴�����[Java Spring](https://ivanhan0511.github.io/post/java-spring/)




## III. IMPORTANT SUPPORT {#chapter-3}
---

### A. Security
**Configure**
���������ʼ��, ע����Щfilter


**Filter**
��������, �������Ӧ����support()��provider����ȥ


**Provider**
�ṩ�Լ�����֤�ȶ�ʵ�ַ���, ����ʵ��`support()`����filter����


**Handler**
���ӳɹ���ʧ�ܵ�handler

#### Permission

#### Protection



### B. Input


### C. Output


### D. Exception


### E. API


### F. Log

### G. Background Job

### H. Cache






## IV. BUSINESS DESIGN {#chapter-4}
---
@RequestBody MultipartFile[] submissions
should be

@RequestParam("file") MultipartFile[] submissions
The files are not the request body, they are part of it and there is no built-in HttpMessageConverter that can convert the request to an array of MultiPartFile.

You can also replace HttpServletRequest with MultipartHttpServletRequest, which gives you access to the headers of the individual parts.

### Controller and input validation
- [ ] Web admin
- [ ] App portal
- [x] Normal `@Valid` and `@Validated`
- [x] DIY `@FlagValidator`




### Service
- [ ] Use MaBatisPlus? How does `mall` use pure MaBatis easily to implement?
- [ ] Simple service, call mapper function directly?
- [ ] Complicate service, call multiple mapper functions?
- [ ] What is the data structure(DTO?) in nternal service?




### DAO Mapper
- [ ] Pure MyBatis or MyBatisPlus?
- [ ] MyBatis PageHelper detail
- [ ] org.springframework.data.domain.PageRequest? Or PearAdmin? Or mall?

**JUST** use MyBatisPLus maven and use default CRUD methods.  
But **REFUSE** to use `QueryWrapper`, use MyBatis' XML mapper.

Reason as below:
- IService��BaseMapper��"�������"��Java�¹涨��Interface������default���÷�, ������CRUDȷʵ�����ظ�д��
- MybatisPlus��QueryWrapperԽ��Խ��Python SQLAlchemy����ORM, ѧϰ�ɱ���, ��һ������Ż����õ�, �����������ʧ
- ���ܺ��ȶ��ԵȲ��ȶ�����Խ��Խ��, ����һ���ֶ���Ԥ, �ܶ���ν��"����"˲��ȫ��, ���ǵ�����MyBatis
- ����MyBatisдXML����ԭ��SQL, ��������SQL��һ����������

**��**�ֲ�ʽ���ݿ�, ���Ƽ�ʹ��ѩ���㷨, ʹ�����ݿ�����ID����


#### Mapper Design
- ���ܲ�ͬ����ѯͬһ����, �ܿ��ܹ�������ά�Ⱥͽ��ά�ȶ�����ͬ, ��˰��չ��ܻ��ֲ�ѯ�б����ݻ��ѯ�������ݱȽϺ���(��ʱ, May 30, 2023)
  - selectOneForXxxFunction(Integer param)
  - selectListForYyyFunction(YyyRequest request)
    - request��ͨ������У����������ϲ�ͬ���������, �Ա�Mapper�����һ�����Ӧ�Զ��controller�㲻ͬ�����Ĳ�ѯ
- ����`<sql/>`���������ݿ���columns�ֶ�
  - Ϊ����`<select/>`�п��ø�����
  - Ϊ��MyBatisCodeHelperPro�������ܹ���`<sql/>`��`<resultMap/>`��Ӧ���ֶ�
- ��ʹ�����ϲ�ѯ, �ᵼ��PageHelper�޷���ȷ��ҳ, ��ʹ���Ӳ�ѯ


### Multiple DB
dehai-admin��Ŀ��ʹ��MS SQL Server �� MySQL �������ݿ�ĵ�������


#### Settings
- application.yml������PageHelper�� helperDialect, ����"mysql"��"sqlserver"�������ݿ���﷨

- ʹ�ö�����Դʱ, ����PageHelperʱҪע��(ֻ����ʹ��application.yml��ʽ�������ļ�ʱ��������):  
  {{< blockquote "application.yml" >}}
  ...
  pagehelper:
    autoRuntimeDialect: true  # �˴������������շ�, ����IDEA�Զ���ʾ��`auto-runtime-dialect: true`
  ...
  {{< /blockquote >}}
  ��Ϊ����շ屻�Զ�ת��Ϊ���߷ָ���, �ᵼ��PageHelper�л�������ԴʱʧЧ


### Transactional
- �������Դ, ���ø�����Դ����, ����Service��ʵ������@DS("customer")ע��ָ������Դ
- �������, ͨ��@DSTransactional ���ݸ���������ָ��������Դ�����л�

#### Propagation
TODO

- ���񴫲�, ��Ҫʱ����봫������ɢҪ��  
  @Transactional(isolation = Isolation.SERIALIZABLE, propagation = Propagation.REQUIRES_NEW)


#### Isolation
��ͬ��"��������"��Spring�ᵽ�ĸ���, ���뼶��������������ݿ������Դ���

| Isolation Level  | ���(Dirty Read) | �����ظ���(Non Repeatable Read) | �ö�(Phantom Read) |
| :---             | :---            | :---                           | :---              |
| Read Uncommitted | Yes             | Yes                            | Yes               |
| Read Committed   | -               | Yes                            | Yes               |
| Repeatable Read  | -               | -                              | Yes               |
| Serializable     | -               | -                              | -                 |

�ܶ������׸�첻���ظ����ͻö���ȷʵ��������Щ����. �������ظ����ص�����update��delete, ���ö����ص�����insert.

�ܵ���˵�ö���������A�����ݽ��в���������B���ǿ�����insert�������ݵģ���Ϊʹ�õ����������������µĸ�������������ǻö���������ʽ�ܶ࣬�Ͳ��о��ˡ�


**TIPs**
{{< alert info >}}
@DS ������� @Transactional ��Ӧ������߷�����  
���� mapper�м���@DS������ @Transactional ���� service �����У���ʱ����ȡĬ�ϵ�datasource
(��Ϊ���������Ѿ���ȡ��һ��datasource��connection������ʱ��DSע��)
{{< /alert >}}

{{< tabbed-codeblock MultiDSTransactional >}}
<!-- tab sInterface -->
public interface SomeService extends IService<Some> {}
<!-- endtab -->

<!-- tab "sImpl" -->
@Service
public class SomeServiceImpl extends ServiceImpl<SomeRepository, Some> implements SomeService {}
<!-- endtab -->

<!-- tab oInterface -->
public interface OtherService extends IService<Other> {}
<!-- endtab -->

<!-- tab oImpl -->
@Service
@DS("db2")
public class OtherServiceImpl extends ServiceImpl<OtherRepository, Other> implements OtherService {}
<!-- endtab -->

<!-- tab combo -->
public interface CombineService {
    Boolean save(CombineRequest request);
}
<!-- endtab -->

<!-- tab comboImpl -->
@Service
public class CombineServiceImpl implements CombineService {
    @Resource
    private SomeService someService;

    @Resource
    private OtherService otherService;

    @Override
    @DSTransactional
    public Boolean save(CombineRequest request) {
        Boolean r1 = saveSome(request);
        Boolean r2 = saveOthers(request);
        return r1 && r2;
    }

    @Transactional(isolation = Isolation.SERIALIZABLE, propagation = Propagation.REQUIRES_NEW)
    protected Boolean saveOthers(CombineRequest request) {
        List<Other> others = methodToGetOthers(request);
        return otherService.saveBatch(others);
    }

    private Boolean saveSome(CombineRequest request) {
        Some some = methodToGetSome(request);
        return someService.save(some);
    }
}
<!-- endtab -->
{{< /tabbed-codeblock >}}


[TODO]:
MyBatis���ֶ��л�, ����������ѧϰ  
And how to `DynamicDataSourceContextHolder.push()` and `DynamicDataSourceContextHolder.poll()`?




### Background Job
#### Quartz





## V. OPERATION {#chapter-5}
---
��Ӫ, �����ƹ�, ��ά, ����, �Զ�������, �Զ����ӿ��ĵ���

### CI

### Monitor


### API DOCUMENT
- [ ] Swagger UI category, description and list
- [ ] With session token




### DEPLOYMENT
#### Docker
- [ ] DockerCompose deployment in single server?


#### JAR




### Others
- ���˶��ĺ�, ��Ϥ���ݵı༭/����, ��ϰд��, ����Ű�/���յȺ��ڼ���
- ��Ӫ, ������������




## VI. APPENDIX {#chapter-6}
---
ժ��������, ��һ������, ������һЩ����, �Ա��Ժ���дһЩ����


### ��д��Ϣ
- DAO: Data Access Object, ���ݷ��ʲ�
- PO: Persistant Object, �־ò����. �������ݿ��ڵ�һ����¼
- DO: Domain Object, �������
- DTO: Data Transfer Object, ͨ����OpenApi���صĶ�����ʹ��DTO, ���ڱ�ṹԭʼ��ֵ{"age":40}
- BO: Business Object, ҵ�����
- VO: Value Object, ���ֶ��� (�����Ҫ, ��ת��Ϊ������ֵ�����, ����{"age":40, "desc":"����֮��"})
- POJO: Plain Old Java Object, ��PO/DO/DTO/BO/VO��ͳ��


### �����淶
- Service/DAO�㷽��������Լ
  + ��ȡ��������ķ�����get��ǰ׺
  + ��ȡ�������ķ�����list��ǰ׺
  + ��ȡͳ��ֵ�ķ�����count��ǰ׺
  + ����ķ�����save/insert��ǰ׺
  + ɾ���ķ�����remove/delete��ǰ׺
  + �޸ĵķ�����edit/update��ǰ׺
- ~~����ģ��������Լ~~
  + ���ݶ���: xxxDO, xxx������
  + ���ݴ������: xxxDTO, xxx��ҵ��������ص�����
  + չʾ����: xxxVO, xxxһ��Ϊ��ҳ����
  + POJO��DO/DTO/BO/VO��ͳ��, ��ֹ������xxxPOJO


