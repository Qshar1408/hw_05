# Домашнее задание к занятию «Использование Terraform в команде»

### Грибанов Антон. FOPS-31

### Цели задания

1. Научиться использовать remote state с блокировками.
2. Освоить приёмы командной работы.


### Чек-лист готовности к домашнему заданию

1. Зарегистрирован аккаунт в Yandex Cloud. Использован промокод на грант.
2. Установлен инструмент Yandex CLI.
3. Любые ВМ, использованные при выполнении задания, должны быть прерываемыми, для экономии средств.

------
### Внимание!! Обязательно предоставляем на проверку получившийся код в виде ссылки на ваш github-репозиторий!
Убедитесь что ваша версия **Terraform** ~>1.8.4
Пишем красивый код, хардкод значения не допустимы!

------
### Задание 0
1. Прочтите статью: https://neprivet.com/
2. Пожалуйста, распространите данную идею в своем коллективе.

------

### Задание 1

1. Возьмите код:
- из [ДЗ к лекции 4](https://github.com/netology-code/ter-homeworks/tree/main/04/src),
- из [демо к лекции 4](https://github.com/netology-code/ter-homeworks/tree/main/04/demonstration1).
2. Проверьте код с помощью tflint и checkov. Вам не нужно инициализировать этот проект.
3. Перечислите, какие **типы** ошибок обнаружены в проекте (без дублей).

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_001.png)

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_002.png)

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_003.png)

#### Ошибки:
* Не был инициализирован проект, соответственно нет установленного Terraform провайдера;
* В коде присутствуют декларированные, но не задействованные переменные;
* В модуле test-vm присутствует ссылка на ветку main без указания конкретного коммита;
* Изменение главной ветки репозитория способно привести к неопределённому поведению кода.
  
------

### Задание 2

1. Возьмите ваш GitHub-репозиторий с **выполненным ДЗ 4** в ветке 'terraform-04' и сделайте из него ветку 'terraform-05'.
2. Повторите демонстрацию лекции: настройте YDB, S3 bucket, yandex service account, права доступа и мигрируйте state проекта в S3 с блокировками. Предоставьте скриншоты процесса в качестве ответа.

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_004.png)

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_005.png)

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_006.png)

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_007.png)

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_008.png)

3. Закоммитьте в ветку 'terraform-05' все изменения.
4. Откройте в проекте terraform console, а в другом окне из этой же директории попробуйте запустить terraform apply.
5. Пришлите ответ об ошибке доступа к state.

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_009.png)

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_010.png)

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_011.png)

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_012.png)

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_013.png)

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_014.png)


6. Принудительно разблокируйте state. Пришлите команду и вывод.

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_015.png)

------
### Задание 3  

1. Сделайте в GitHub из ветки 'terraform-05' новую ветку 'terraform-hotfix'.
2. Проверье код с помощью tflint и checkov, исправьте все предупреждения и ошибки в 'terraform-hotfix', сделайте коммит.

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_016.png)

3. Откройте новый pull request 'terraform-hotfix' --> 'terraform-05'. 
4. Вставьте в комментарий PR результат анализа tflint и checkov, план изменений инфраструктуры из вывода команды terraform plan.
5. Пришлите ссылку на PR для ревью. Вливать код в 'terraform-05' не нужно.
#### Ссылка на ревью: [Ссылка на ревью](https://github.com/Qshar1408/hw_05/commit/2fee5a089b60b720e22f6de2410fa515c69ac5ca)

------
### Задание 4

1. Напишите переменные с валидацией и протестируйте их, заполнив default верными и неверными значениями. Предоставьте скриншоты проверок из terraform console. 

- type=string, description="ip-адрес" — проверка, что значение переменной содержит верный IP-адрес с помощью функций cidrhost() или regex(). Тесты:  "192.168.0.1" и "1920.1680.0.1";
- type=list(string), description="список ip-адресов" — проверка, что все адреса верны. Тесты:  ["192.168.0.1", "1.1.1.1", "127.0.0.1"] и ["192.168.0.1", "1.1.1.1", "1270.0.0.1"].

#### Решение:

```bash
variable "ip_address" {

  description = "ip-адрес"

  type        = string

  default = "192.168.0.1"

  validation {

    condition     = can(regex("^((25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])\\.){3}(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])$", var.ip_address))

    error_message = "Неправильный ip-адрес"

  }

}



variable "ip_address_list" {

  description = "список ip-адресов"

  type        = list(string)

  default     = ["192.168.0.1", "1.1.1.1", "127.0.0.1"]

  validation {

    condition = alltrue([for ip in var.ip_address_list: can(regex("^((25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])\\.){3}(25[0-5]|2[0-4][0-9]|1[0-9]{2}|[1-9]?[0-9])$", ip))])

    error_message = "Неправильный список ip-адресов"

  }

}
```


   * Если в адресах нет ошибок, то terraform console выведет пустой результат.
   * Если в адресах есть ошибки, то terraform console выведет результат валидации:
     
![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_017.png)


## Дополнительные задания (со звёздочкой*)

**Настоятельно рекомендуем выполнять все задания со звёздочкой.** Их выполнение поможет глубже разобраться в материале.   
Задания со звёздочкой дополнительные, не обязательные к выполнению и никак не повлияют на получение вами зачёта по этому домашнему заданию. 
------
### Задание 5*
1. Напишите переменные с валидацией:
- type=string, description="любая строка" — проверка, что строка не содержит символов верхнего регистра;
- type=object — проверка, что одно из значений равно true, а второе false, т. е. не допускается false false и true true:
```
variable "in_the_end_there_can_be_only_one" {
    description="Who is better Connor or Duncan?"
    type = object({
        Dunkan = optional(bool)
        Connor = optional(bool)
    })

    default = {
        Dunkan = true
        Connor = false
    }

    validation {
        error_message = "There can be only one MacLeod"
        condition = <проверка>
    }
}
```

#### Решение

```bash
variable "string_check" {
  type        = string
  description = "любая строка"
  default     = "test-string"

  validation {
      condition = var.string_check == lower(var.string_check)
      error_message = "В строке не должно быть символов верхнего регистра"
  }
}


variable "in_the_end_there_can_be_only_one" {
    description="Who is better Connor or Duncan?"
    type = object({
        Dunkan = optional(bool)
        Connor = optional(bool)
    })

    default = {
        Dunkan = true
        Connor = false
    }

    validation {
        error_message = "There can be only one MacLeod"
        condition = var.in_the_end_there_can_be_only_one.Dunkan !=  var.in_the_end_there_can_be_only_one.Connor
    }
}
```

   * Если в валидации нет ошибок, то terraform console выведет пустой результат.
   * Если в строке будут символы верхнего регистра, то увидим ошибку в terraform console:

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_018.png)

   * Если значение Dunkan и Connor будет true или false, то увидим ошибку в terraform console:

![hw_05](https://github.com/Qshar1408/hw_05/blob/main/img/hw_05_019.png)

------

### Правила приёма работы

Ответы на задания и необходимые скриншоты оформите в md-файле в ветке terraform-05.

В качестве результата прикрепите ссылку на ветку terraform-05 в вашем репозитории.

**Важно.** Удалите все созданные ресурсы.

### Критерии оценки

Зачёт ставится, если:

* выполнены все задания,
* ответы даны в развёрнутой форме,
* приложены соответствующие скриншоты и файлы проекта,
* в выполненных заданиях нет противоречий и нарушения логики.

На доработку работу отправят, если:

* задание выполнено частично или не выполнено вообще,
* в логике выполнения заданий есть противоречия и существенные недостатки. 

