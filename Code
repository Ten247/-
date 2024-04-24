import speech_recognition  
import pyttsx3 
import webbrowser  
import traceback  
import wikipediaapi 
import random  
from deep_translator import GoogleTranslator 
import subprocess 
"""Инициализация синтеза речи"""
tts = pyttsx3.init()
voices = tts.getProperty("voices")
tts.setProperty("voice", "ru")
for voice in voices:
    ru = voice.id.find("RHVoice\Anna")  
    if ru > -1:  
        tts.setProperty("voice", voice.id)
def play_voice_assistant_speech(text_to_speech):
    """
    :param text_to_speech: текст, который нужно преобразовать в
    речь
    """
    tts.say(str(text_to_speech))
    tts.runAndWait()
def record_and_recognize_audio(*args: tuple):
    """
    Запись и распознавание аудио
    """
    with microphone:
        recognized_data = ""
        recognizer.adjust_for_ambient_noise(microphone, duration=2)
        try:
            print("Listening...")
            audio = recognizer.listen(microphone, 10, 15)
        except speech_recognition.WaitTimeoutError:
            print("Can you check if your microphone is on, please?")
            return
        try:
            print("Started recognition...")
            recognized_data = recognizer.recognize_google(
            	audio, language="ru"
            ).lower()
        except speech_recognition.UnknownValueError:
            pass
        """
	    в случае проблем с доступом в Интернет происходит выброс
        ошибки
	    """
        except speech_recognition.RequestError:
            print("Check your Internet Connection, please")
            play_voice_assistant_speech(
		  	"Пожалуйста, проверьте соединение с Интернетом!"
		  )
        return recognized_data
Для обработки исключений здесь используется конструкция with ─ try ─ except. 
Для запуска базовой программы напишем main-функции:
if __name__ == "__main__":
    recognizer = speech_recognition.Recognizer()
    microphone = speech_recognition.Microphone()

    while True:
        voice_input = record_and_recognize_audio()
        print('Пользователь:', voice_input)
2.2 Навыки ассистента
commands = {
    ("подбрось", "heads"): flip_a_coin,
    ("hello", "hi", "morning", "привет", "здорова", "хэй"): play_greetings,
    ("bye", "goodbye", "quit", "exit", "stop", "пока", "хватит", "стоп"): play_farewell_and_quit,
    ("victoria", "help", "вика", "виктория", "помощь"): name_trigger,
    ("search", "google", "find", "найди", "погода", "прогноз", "гугл", "интернет", "интернете"): search_for_term_on_google,
    ("video", "youtube", "watch", "видео", "ютуб"): search_for_video_on_youtube,
    ("wikipedia", "definition", "about", "определение", "википедия", "википедии"): search_for_definition_on_wikipedia,
    ("translate", "interpretation", "translation", "перевод", "перевести", "переведи", "переводчик"): get_translation,
}
print(
    "Доступные команды: ",
    "Приветствие:                Привет",
    "Помощь (выводит это меню):  Помощь; Виктория",
    "Закончить разговор:         Пока; Хватит; Стоп",
    "Подбросить монетку:         Подбрось монетку; Heads or tails",
    "Запустить переводчик:       Перевод; Перевести; Переведи;",
    "Искать в Google:            Найди; гугл; <запрос>",
    "Искать в Википедии:         Найди в википедии; <запрос>",
    "Искать в Ютуб:              ютуб; youtube <запрос>",
sep="\n)
def execute_command_with_name(command_name: str, *args: list):
    """
    :param command_name: название команды
    :param args: аргументы, которые будут переданы в функцию
    :return:
    """
    for key in commands.keys():
        if command_name in key:
            commands[key](*args)
        else:
            pass
if __name__ == "__main__":
    microphone = speech_recognition.Microphone()
    while True:
        voice_input = record_and_recognize_audio()
        print('Пользователь:', voice_input)

        voice_input = voice_input.split(" ")
        command = voice_input[0]
        command_options = [
str(input_part) for input_part in voice_input[1:len(voice_input)]
	    ]
        execute_command_with_name(command, command_options)
def search_for_video_on_youtube(*args: tuple):
    """
    :param args: фраза поискового запроса
    """
    if not args[0]: return
    search_term = " ".join(args[0])
    url = "https://www.youtube.com/results?search_query=" + search_term
    webbrowser.get().open(url)
    print("Вот что я нашла по запросу " + search_term + " в ютуб.")
    play_voice_assistant_speech("Вот что я нашла по запросу " + search_term + " в ютуб.")

Поиск в Google:
def search_for_term_on_google(*args: tuple):
    """
    :param args: фраза поискового запроса
    """
    if not args[0]: return
    search_term = " ".join(args[0])
    url = "https://google.com/search?q=" + search_term
    webbrowser.get().open(url)
    print("Вот что я нашла по запросу {}.".format(search_term))
    play_voice_assistant_speech("Вот что я нашла по запросу {}.".format(search_term))
def search_for_definition_on_wikipedia(*args: tuple):
    """
    :param args: фраза поискового запроса
    """
    if not args[0]: return
    search_term = " ".join(args[0])
    wiki = wikipediaapi.Wikipedia("ru")
    wiki_page = wiki.page(search_term)
    try:
        if wiki_page.exists():
            print("Вот что я нашла по запросу {} в Википедии".format(search_term))
            play_voice_assistant_speech("Вот что я нашла по запросу {} в википедии".format(search_term))
            webbrowser.get().open(wiki_page.fullurl)
            play_voice_assistant_speech(wiki_page.summary.split(".")[:2])
        else:
            print("Такой информации нет в Википедии, вот результаты в google.")
            play_voice_assistant_speech("Такой информации нет в Википедии, вот результаты в google.")
            url = "https://google.com/search?q=" + search_term
            webbrowser.get().open(url)
    except:
        print("Seems like we have a trouble. See logs for more information")
        play_voice_assistant_speech("Seems like we have a trouble. See logs for more information")
        traceback.print_exc()
        return
Подбросить монетку:
def flip_a_coin(*args: tuple):
    l = random.randint(0, 2)
    if l == 0:
        print("Орёл!")
        play_voice_assistant_speech("Орёл!")
    else:
        print("Решка!")
        play_voice_assistant_speech("Решка!")
    return
Помощь:
def name_trigger(*args: tuple):
    print("Чем я могу помочь?")
    play_voice_assistant_speech("Чем я могу пом+очь?")
    print(
    "Доступные команды: ",
    "Приветствие:                Привет",
    "Помощь (выводит это меню):  Помощь; Виктория",
    "Закончить разговор:         Пока; Хватит; Стоп",
    "Подбросить монетку:         Подбрось монетку; Heads or tails",
    "Запустить переводчик:       Перевод; Перевести; Переведи;",
    "Искать в Google:            Найди; гугл; <запрос>",
    "Искать в Википедии:         Найди в википедии; <запрос>",
    "Искать в Ютуб:              ютуб; youtube <запрос>",
sep="\n")
Переводчик:
def get_translation(*args: tuple):
    """
    Переводчик
    :param args:
    :return:
    """
    print("Запускаю навык 'Перевод'!")
    play_voice_assistant_speech("Запускаю навык 'Перевод'!")
    play_voice_assistant_speech("Говорите целевой язык.")
    lang = record_and_recognize_audio()
    if lang == "русский":
        target = "ru"
    else:
        target = "en"
    play_voice_assistant_speech("Говорите фразу для перевода.")
    to_translate = record_and_recognize_audio()
    translated = GoogleTranslator(source='auto', target=target).translate(to_translate)
    print(translated)
    play_voice_assistant_speech(translated)
    return
Поздороваться с пользователем:
def play_greetings(*args: tuple):
    """
    Приветствие пользователя
    """
    print("Привет, пользователь! Я - голосовой помощник Виктория. Чем я могу помочь вам?")
    play_voice_assistant_speech("Привет, пользователь! Я - голосовой помощник Виктория. Чем я могу пом+очь вам?")
Завершение программы:
def play_farewell_and_quit(*args: tuple):
    """
    Проигрывание прощальной речи и выход
    """
    print("До свидания! Хорошего дня!")
    play_voice_assistant_speech("До свидания! Хорошего дня!")
    tts.stop()
    quit()
Открытие базовых программ:
def open_app(*args: tuple):
    """
    Отрыть одну из стандартных программ Windows
    """
    apps = {
        ("блокнот", "notepad"): "notepad",
        ("калькулятор", "calculator"): "calc",
        ("браузер", "browser"): "browser",
        ("wordpad", "вордпад"): "write",
        ("пэйнт", "paint"): "mspaint",
        ("проводник", "explorer"): "explorer"
    }
    args = ''.join(args[0])
    error = 0
    for key in apps.keys():
        if args in key:
            if apps[key] != "browser":
                subprocess.Popen(apps[key])
                print(f'Запускаю {args}...')
                play_voice_assistant_speech(f'Запускаю {args}...')
                break
            else:
                webbrowser.open('https://google.com')
                print(f'Запускаю {args}...')
                play_voice_assistant_speech(f'Запускаю {args}...')
                break
        else: error += 1
    if error >= len(apps):
        print('Такой программы не найдено...')
        play_voice_assistant_speech('Такой программы не найдено.')
