---
layout: post
title: "[Заметка] Генерируем Entity из базы данных"
date: "2017-12-22"
category:
  - it
  - prog
---
{% include image.html src="/assets/it/prog/doctrine-entities.jpg" %}

При использовании микросервисной архитектуры, генерирование сущностей - очень частая задача. Каждый новый проект требует делать это заново, так как, как правило, каждый проект имеет собственную БД. Сейчас я постоянно пользуюсь поиском, чтобы вспомнить как это делается с помощью кодогенерации.

<!--more-->

Как сгенерировать сущности Doctrine из существующей базы.
- Выполняем команду создания классов на основе описания базы данных.
```
bin/console doctrine:mapping:convert annotation ./src/BundleName/Entity --from-database --filter="EntityNameOptional"
```

- В сгенерированных сущностях добавляем неймспейсы.
- Генерируем сеттеры и геттеры.
```
bin/console doctrine:generate:entities BundleName:EntityNameOptional --no-backup
```

Связи один ко многим следует прописывать отдельно с помощью аннотаций:

{%- highlight php -%}
<?php

namespace AutoBundle\Entity;

use Doctrine\Common\Collections\ArrayCollection;
use Doctrine\Common\Collections\Collection;
use Doctrine\ORM\Mapping as ORM;

/**
 * @ORM\Table(name="car")
 * @ORM\Entity(repositoryClass="AutoBundle\Entity\Repository\Car")
 */
class Car
{
    /**
     * @var int
     *
     * @ORM\Column(name="id", type="bigint", nullable=false)
     * @ORM\Id
     * @ORM\GeneratedValue(strategy="SEQUENCE")
     * @ORM\SequenceGenerator(sequenceName="car_id_seq", allocationSize=1, initialValue=1)
     */
    private $id;

    /**
     * @var Collection
     *
     * @ORM\OneToMany(targetEntity="AutoBundle\Entity\Wheel", mappedBy="car", fetch="LAZY", cascade={"persist", "remove"})
     */
    private $wheelList;

    public function __construct()
    {
        $this->wheelList = new ArrayCollection();
    }
}

/**
 * @ORM\Table(name="wheel")
 * @ORM\Entity(repositoryClass="AutoBundle\Entity\Repository\Wheel")
 */
class Wheel
{
    /**
     * @var int
     *
     * @ORM\Column(name="id", type="bigint", nullable=false)
     * @ORM\Id
     * @ORM\GeneratedValue(strategy="SEQUENCE")
     * @ORM\SequenceGenerator(sequenceName="wheel_id_seq", allocationSize=1, initialValue=1)
     */
    private $id;

    /**
     * @var Car
     *
     * @ORM\ManyToOne(targetEntity="Car", cascade={"persist"}, inversedBy="wheelList")
     * @ORM\JoinColumns({
     *   @ORM\JoinColumn(name="car_id", referencedColumnName="id", onDelete="CASCADE")
     * })
     */
    private $car;
}
{%- endhighlight -%}

В данном случае при создании нового автомобиля можно добавить ему шины, которые будут автоматически сохранены при persist автомобиля. А при удалении автомобиля, связаные колеса будут каскадно удалены.
