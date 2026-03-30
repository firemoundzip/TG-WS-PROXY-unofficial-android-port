# TG-WS-PROXY-unofficial-android-port
Как работает приложение — кратко
TG WS Proxy — это локальный MTProto-прокси для Telegram, который обходит блокировки через WebSocket.

Принцип работы
Telegram (на телефоне) 
    ↓ MTProto через 127.0.0.1:1443
Прокси-приложение (Android)
    ↓ WebSocket через TLS (wss://kws*.web.telegram.org)
Серверы Telegram
Что происходит внутри
Запуск прокси — приложение открывает локальный сервер на порту 1443
Настройка Telegram — копируешь tg://proxy?server=127.0.0.1&port=1443&secret=dd... и открываешь в Telegram
Подключение — Telegram подключается к локальному прокси
Handshake — прокси расшифровывает 64-байтный handshake, извлекает номер DC (дата-центра)
WebSocket — прокси устанавливает WebSocket-соединение к Telegram DC через домен kws*.web.telegram.org (обходит DPI)
Мост — прокси перешифровывает данные между Telegram-клиентом и DC:
От клиента: расшифровка → перешифровка → отправка через WS
От DC: получение через WS → расшифровка → перешифровка → отправка клиенту
Зачем это нужно
Обход блокировок — провайдеры блокируют прямые IP Telegram, но пропускают WebSocket к *.web.telegram.org
Локально — работает на твоём телефоне, не нужны внешние серверы
Пул соединений — заранее открывает WS-соединения для быстрого подключения
Технические детали
Шифрование: AES-256-CTR (как в оригинальном MTProto Proxy)
Протоколы: поддерживает abridged, intermediate, padded intermediate
Fallback: если WebSocket не работает → переключается на прямой TCP
Всё работает в фоновом режиме через Android Foreground Service.
ПРОЕКТ СВЯЗАН С TG WS PROXY - ОТ Flowseal
ССЫЛКА НА ОРИГИНАЛ - https://github.com/Flowseal/tg-ws-proxy
НЕ КИДАЙТЕ СТРАЙКИ ( пожалуйста ) -_-
