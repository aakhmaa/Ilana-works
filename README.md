# Ilana-works
It is a homework
from typing import List, Optional
from src.animal import Animal


class Zoo:
    """
    Класс Zoo, управляющий коллекцией животных.
    """

    def _init_(self, name: str, location: str) -> None:
        """
        Инициализация зоопарка.
        
        Аргументы:
            name: Название зоопарка
            location: Локация зоопарка
        """
        self._name = name
        self._location = location
        self._animals: List[Animal] = []


    """Основные тесты для проекта OOP."""

import unittest
from src.animals import Dog, Cat, Bird
from src.zoo import Zoo


class TestAnimal(unittest.TestCase):
    """Тесты для классов животных."""

    def setUp(self) -> None:
        """Set up test fixtures."""
        self.dog = Dog("Max", 3, "Labrador")
        self.cat = Cat("Luna", 2, "Gray")
        self.bird = Bird("Polly", 1, "Parrot")

    def test_dog_initialization(self) -> None:
        """Тестирование инициализации собаки."""
        self.assertEqual(self.dog.name, "Max")
        self.assertEqual(self.dog.age, 3)
        self.assertEqual(self.dog.breed, "Labrador")

    def test_dog_sound(self) -> None:
        """Тестирование звука собаки."""
        self.assertIn("Woof", self.dog.make_sound())

    def test_dog_tricks(self) -> None:
        """Тестирование обучения трюкам."""
        self.dog.teach_trick("Fetch")
        self.assertIn("performed", self.dog.perform_trick("Fetch"))

    def test_cat_sound(self) -> None:
        """Тестирование звука кота."""
        self.assertIn("Meow", self.cat.make_sound())

    def test_cat_eat(self) -> None:
        """Тестирование коконмления кота."""
        self.cat.exercise(20)  # Уменьшить энергию
        initial_energy = self.cat.energy
        self.cat.eat("fish")
        self.assertGreater(self.cat.energy, initial_energy)

    def test_bird_fly(self) -> None:
        """Тестирование полета птицы."""
        result = self.bird.fly(50)
        self.assertIn("flew", result)
        self.assertEqual(self.bird.altitude, 50)

    def test_bird_cannot_fly(self) -> None:
        """Тестирование нелетающей птицы."""
        penguin = Bird("Pingu", 5, "Penguin", can_fly=False)
        result = penguin.fly(100)
        self.assertIn("cannot fly", result)

    def test_sleep(self) -> None:
        """Тестирование функции сна."""
        self.dog.energy  # Store reference
        self.dog.exercise(30)  # Reduce energy
        self.dog.sleep(2)  # Restore energy
        self.assertGreater(self.dog.energy, 40)

    def test_info_string(self) -> None:
        """Тестирование генерации строки информации."""
        info = self.dog.get_info()
        self.assertIn("Max", info)
        self.assertIn("Dog", info)


class TestZoo(unittest.TestCase):
    """Тесты для класса Zoo."""

    def setUp(self) -> None:
        """Подготовка тестовых ресурсов."""
        self.zoo = Zoo("Test Zoo", "TestCity")
        self.dog = Dog("Buddy", 4, "Beagle")
        self.cat = Cat("Mittens", 1, "White")

    def test_zoo_initialization(self) -> None:
        """Тестирование инициализации зоопарка."""
        self.assertEqual(self.zoo.name, "Test Zoo")
        self.assertEqual(self.zoo.location, "TestCity")
        self.assertEqual(self.zoo.get_animal_count(), 0)

    def test_add_animal(self) -> None:
        """Тестирование добавления животных."""
        self.zoo.add_animal(self.dog)
        self.assertEqual(self.zoo.get_animal_count(), 1)

    def test_remove_animal(self) -> None:
        """Тестирование от удаления животных."""
        self.zoo.add_animal(self.dog)
        result = self.zoo.remove_animal(self.dog)
        self.assertTrue(result)
        self.assertEqual(self.zoo.get_animal_count(), 0)

    def test_find_animal_by_name(self) -> None:
        """Тестирование поиска животного по имени."""
        self.zoo.add_animal(self.dog)
        found = self.zoo.find_animal_by_name("Buddy")
        self.assertEqual(found, self.dog)

    def test_species_count(self) -> None:
        """Тестирование посчета видов."""
        self.zoo.add_animal(self.dog)
        self.zoo.add_animal(self.cat)
        self.assertEqual(self.zoo.get_species_count("Dog"), 1)
        self.assertEqual(self.zoo.get_species_count("Cat"), 1)

    def test_get_all_sounds(self) -> None:
        """Тестирование получения всех звуков."""
        self.zoo.add_animal(self.dog)
        self.zoo.add_animal(self.cat)
        sounds = self.zoo.get_all_sounds()
        self.assertEqual(len(sounds), 2)

    def test_animals_property(self) -> None:
        """Тестирование тоже свойство возвращает копию."""
        self.zoo.add_animal(self.dog)
        animals_copy = self.zoo.animals
        self.assertEqual(len(animals_copy), 1)
        # Проверить, что это копия, а не исходный список
        animals_copy.append(self.cat)
        self.assertEqual(self.zoo.get_animal_count(), 1)


if _name_ == "_main_":
    unittest.main()



"""Конкретные реализации животных, демонстрирующие наследование и полиморфизм."""

from typing import Optional
from src.animal import Animal


class Dog(Animal):
    """
    Класс собаки, демонстрирующий конкретную реализацию базового класса Animal.
    
    Этот класс демонстрирует:
    - Наследование от абстрактного базового класса
    - Реализацию абстрактных методов
    - Полиморфизм
    """

    def _init_(self, name: str, age: int, breed: str) -> None:
        """
        Инициализация собаки.
        
        Аргументы:
            name: Имя собаки
            age: Возраст собаки
            breed: Порода собаки
        """
        super()._init_(name, age, "Dog")
        self._breed = breed
        self._tricks: list[str] = []

    @property
    def breed(self) -> str:
        """Получить породу собаки."""
        return self._breed

    def make_sound(self) -> str:
        """Вернуть звук, который издает собака."""
        return f"{self._name} says: Woof! Woof!"

    def eat(self, food: str) -> None:
        """Накормить собаку."""
        if self._energy < 100:
            self._energy = min(100, self._energy + 20)
            print(f"{self._name} съел {food} и очень энергична!")
        else:
            print(f"{self._name} сейчас не голодна.")

    def teach_trick(self, trick: str) -> None:
        """
        Научить собаку новому трюку.
        
        Аргументы:
            trick: Название трюка
        """
        if trick not in self._tricks:
            self._tricks.append(trick)
            print(f"{self._name} научился новому трюку: {trick}!")

    def perform_trick(self, trick: str) -> Optional[str]:
        """
        Выполнить изученный трюк.
        
        Аргументы:
            trick: Трюк для выполнения
            
        Возвращает:
            Описание выполнения трюка
        """
        if self._energy < 10:
            return f"{self._name} is too tired to perform tricks."
        elif trick in self._tricks:
            self._energy -= 10
            return f"{self._name} performed the trick: {trick}!"
        else:
            return f"{self._name} hasn't learned the {trick} trick yet."

    def get_info(self) -> str:
        """Получить подробную информацию о собаке."""
        base_info = super().get_info()
        tricks_info = f", Трюки: {', '.join(self._tricks) if self._tricks else 'Нет'}"
        return f"{base_info}, Порода: {self._breed}{tricks_info}"


class Cat(Animal):
    """Класс кота, демонстрирующий другую конкретную реализацию Animal."""

    def _init_(self, name: str, age: int, color: str) -> None:
        """
        Инициализация кота.
        
        Аргументы:
            name: Имя кота
            age: Возраст кота
            color: Окраска/пыстэрн кота
        """
        super()._init_(name, age, "Cat")
        self._color = color
        self._mood = "neutral"

    @property
    def color(self) -> str:
        """Получить окраску кота."""
        return self._color

    @property
    def mood(self) -> str:
        """Получить унастроение кота."""
        return self._mood

    def make_sound(self) -> str:
        """Вернуть звук, который издает кот."""
        return f"{self._name} says: Meow!"

    def eat(self, food: str) -> None:
        """Накормить кота."""
        if self._energy < 100:
            self._energy = min(100, self._energy + 15)
            self._mood = "happy"
            print(f"{self._name} съел {food} и теперь очень рад!")
        else:
            print(f"{self._name} сейчас не интересуются в еде.")

    def scratch(self, target: str = "furniture") -> str:
        """
        Кот царапает что-нибудь.
        
        Аргументы:
            target: Объект, который царапает кот
            
        Возвращает:
            Описание акции царапания
        """
        self._mood = "playful"
        return f"{self._name} царапает {target}!"

    def get_info(self) -> str:
        """Получить подробную информацию о коте."""
        base_info = super().get_info()
        return f"{base_info}, Окраска: {self._color}, Унастроение: {self._mood}"


class Bird(Animal):
    """Класс птицы, демонстрирующий еще одну конкретную реализацию Animal."""

    def _init_(self, name: str, age: int, species: str, can_fly: bool = True) -> None:
        """
        Инициализация птицы.
        
        Аргументы:
            name: Имя птицы
            age: Возраст птицы
            species: Вид птицы
            can_fly: Может ли птица летать
        """
        super()._init_(name, age, species)
        self._can_fly = can_fly
        self._altitude = 0

    @property
    def can_fly(self) -> bool:
        """Проверить, может ли птица летать."""
        return self._can_fly

    @property
    def altitude(self) -> int:
        """Получить текущую высоту птицы."""
        return self._altitude

    def make_sound(self) -> str:
        """Вернуть звук, который издает птица."""
        return f"{self._name} says: Tweet! Tweet!"

    def eat(self, food: str) -> None:
        """Накормить птицу."""
        if self._energy < 100:
            self._energy = min(100, self._energy + 10)
            print(f"{self._name} съел {food} и чувствует себя энергичным!")
        else:
            print(f"{self._name} имеет достаточно энергии.")

    def fly(self, altitude: int) -> Optional[str]:
        """
        Дать птице летать на определенной высоте.
        
        Аргументы:
            altitude: Целевая высота в метрах
            
        Возвращает:
            Описание полета или ничего, если не вы могли летать
        """
        if not self._can_fly:
            return f"{self._name} cannot fly."
        elif self._energy < 20:
            return f"{self._name} is too tired to fly."
        else:
            self._altitude = altitude
            self._energy -= 20
            return f"{self._name} flew to {altitude} meters altitude!"

    def land(self) -> str:
        """На землю птице."""
        self._altitude = 0
        return f"{self._name} has landed!"

    def get_info(self) -> str:
        """Получить подробную информацию о птице."""
        base_info = super().get_info()
        fly_status = "Да" if self._can_fly else "Нет"
        return f"{base_info}, Может летать: {fly_status}, Высота: {self._altitude}м"


    
    """Базовый класс Animal, демонстрирующий наследование и абстракцию."""

from abc import ABC, abstractmethod
from typing import Optional


class Animal(ABC):
    """
    Абстрактный базовый класс для всех животных.
    
    Этот класс демонстрирует:
    - Инкапсуляцию (приватные атрибуты со свойствами)
    - Наследование (базовый класс для подклассов)
    - Абстракцию (абстрактные методы)
    """

    def _init_(self, name: str, age: int, species: str) -> None:
        """
        Инициализация животного.
        
        Аргументы:
            name: Имя животного
            age: Возраст животного в годах
            species: Вид животного
        """
        self._name = name
        self._age = age
        self._species = species
        self._energy = 100  # Уровень энергии от 0-100

    @property
    def name(self) -> str:
        """Получить имя животного."""
        return self._name

    @property
    def age(self) -> int:
        """Получить возраст животного."""
        return self._age

    @property
    def species(self) -> str:
        """Получить вид животного."""
        return self._species

    @property
    def energy(self) -> int:
        """Получить уровень энергии животного."""
        return self._energy

    @abstractmethod
    def make_sound(self) -> str:
        """
        Вернуть звук, который издает животное.
        
        Это абстрактный метод, который должен быть реализован подклассами.
        """
        pass

    @abstractmethod
    def eat(self, food: str) -> None:
        """
        Накормить животное.
        
        Аргументы:
            food: Тип пищи для животного
        """
        pass

    def sleep(self, hours: int) -> None:
        """
        Уложить животное спать.
        
        Аргументы:
            hours: Количество часов сна
        """
        energy_gain = min(hours * 10, 100 - self._energy)
        self._energy += energy_gain
        print(f"{self._name} спал {hours} час(ов) и получил {energy_gain} энергии.")

    def exercise(self, minutes: int) -> None:
        """
        Физическая нагрузка для животного.
        
        Аргументы:
            minutes: Продолжительность упражнения в минутах
        """
        energy_loss = min(minutes // 5, self._energy)
        self._energy -= energy_loss
        print(f"{self._name} занимался {minutes} минут и потерял {energy_loss} энергии.")

    def get_info(self) -> str:
        """Получить подробную информацию о животном."""
        return (f"Имя: {self._name}, Вид: {self._species}, "
                f"Возраст: {self._age} лет, Энергия: {self._energy}/100")

    def _str_(self) -> str:
        """Вернуть строковое представление животного."""
        return f"{self._name} - {self._species}"

    def _repr_(self) -> str:
        """Вернуть детальное строковое представление."""
        return f"{self._class.name_}(name='{self._name}', age={self._age})"






        Простой OOP проект - чистый проект Python, демонстрирующий принципы ООП.

Этот пакет содержит хорошо структурированные классы и модули, следуя лучшим практикам Python.
"""

_version_ = "1.0.0"
_author_ = "Ваше имя"
_all_ = ["Animal", "Dog", "Cat", "Bird", "Zoo"]





"""Основное приложение, демонстрирующее принципы OOP."""

from src.animals import Dog, Cat, Bird
from src.zoo import Zoo


def main() -> None:
    """Зарустить основное приложение."""
    print("=" * 60)
    print("Каплас в систему управления OOP Zoo Management!")
    print("=" * 60)

    # Создание зоопарка
    zoo = Zoo("Парк Sunshine", "Центральный город")
    print(f"\nСоздано: {zoo}\n")

    # Создание различных животных
    print("--- Создание Животных ---")
    dog = Dog("Rex", 5, "Golden Retriever")
    print(dog)

    cat = Cat("Whiskers", 3, "Orange Tabby")
    print(cat)

    bird1 = Bird("Tweety", 2, "Canary")
    print(bird1)

    bird2 = Bird("Flightless", 10, "Penguin", can_fly=False)
    print(bird2)

    # Добавление животных в зоопарк
    print("\n--- Добавление Животных в Парк ---")
    for animal in [dog, cat, bird1, bird2]:
        zoo.add_animal(animal)

    # Отображение всех животных
    zoo.display_animals()

    # Животные, издающие звуки
    print("\n--- Животные звуки ---")
    for sound in zoo.get_all_sounds():
        print(sound)

    # Накормление всех животных
    zoo.feed_all()

    # Обучение собаки
    print("\n--- Обучение собаки ---")
    dog.teach_trick("Sit")
    dog.teach_trick("Stay")
    print(dog.perform_trick("Sit"))
    print(dog.perform_trick("Dance"))

    # активность кота
    print("\n--- Активности кота ---")
    print(cat.scratch("sofa"))
    cat.sleep(2)

    # Полет птицы
    print("\n--- Полет птицы ---")
    print(bird1.fly(100))
    print(bird1.land())
    print(bird2.fly(50))  # Пингвин не может летать

    # Физическая нагружка всех животных
    zoo.exercise_all(20)

    # Отображение обновленной информации
    zoo.display_animals()

    # Статистика
    print("\n--- Статистика Парка ---")
    print(f"Всего животных: {zoo.get_animal_count()}")
    print(f"Собаки: {zoo.get_species_count('Dog')}")
    print(f"Коты: {zoo.get_species_count('Cat')}")
    print(f"Птицы: {zoo.get_species_count('Bird')}")

    # Поиск определенного животного
    print("\n--- Поиск Животных ---")
    found = zoo.find_animal_by_name("Whiskers")
    if found:
        print(f"Найдено: {found.get_info()}")

    print("\n" + "=" * 60)
    print("Спасибо за посещение Парка OOP!")
    print("=" * 60)


if _name_ == "_main_":
    main()
