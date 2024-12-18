﻿# weatherbot


1. Импорт необходимых модулей:
      import logging
   import requests
   from aiogram import Bot, Dispatcher, executor, types
   
   Здесь мы импортируем модули для логирования, работы с HTTP-запросами и для создания Telegram-бота.

2. Константы:
      TELEGRAM_TOKEN = '7054445469:AAFLAY3j9bn-2RlocorbyklmJYcz80sPTtY'
   API_KEY = '991ae0b98e52ba6bf2df8ea35246eb50'
   CITY = 'Samara'
   
   Здесь мы задаем токен бота, ключ API для доступа к погодному сервису и название города, для которого будет запрашиваться погода.

3. Создание бота и диспетчера:
      bot = Bot(token=TELEGRAM_TOKEN)
   dp = Dispatcher(bot)
   
   Мы создаем экземпляр бота и диспетчера, который будет обрабатывать входящие сообщения.

4. Функция для получения погоды:
      def get_weather(city):
       URL = f'http://api.openweathermap.org/data/2.5/weather?q={city}&appid={API_KEY}&units=metric&lang=ru'
       response = requests.get(URL)
   
   Эта функция формирует URL для запроса погоды и отправляет HTTP-запрос.

5. Обработка ответа:
      if response.status_code == 200:
       data = response.json()
       main = data['main']
       weather = data['weather'][0]
   
   Если запрос успешен (код 200), мы получаем данные в формате JSON и извлекаем информацию о температуре, давлении, влажности и описании погоды.

6. Форматирование ответа:
      return f'Погода в городе {city}:\\n' \
          f'Температура: {temp}°C\\n' \
          f'Давление: {pressure} гПа\\n' \
          f'Влажность: {humidity}%\\n' \
          f'Описание: {description.capitalize()}'
   
   Функция возвращает строку с информацией о погоде в удобном формате.

7. Обработка команд:
      @dp.message_handler(commands=['start'])
   async def start_command(message: types.Message):
       await message.answer('Здравствуйте! Напишите /weather, чтобы узнать погоду в Самаре.')
   
   Эта функция обрабатывает команду /start и отправляет приветственное сообщение.

8. Команда для получения погоды:
      @dp.message_handler(commands=['weather'])
   async def weather_command(message: types.Message):
       weather_info = get_weather(CITY)
       await message.answer(weather_info)

9. Запуск бота:
      if __name__ == '__main__':
       executor.start_polling(dp, skip_updates=True)
   
   
