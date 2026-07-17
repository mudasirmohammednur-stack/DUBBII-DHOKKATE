from kivy.app import App
from kivy.uix.screenmanager import ScreenManager, Screen
from kivy.uix.boxlayout import BoxLayout
from kivy.uix.button import Button
from kivy.uix.label import Label


class HomeScreen(Screen):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)

        layout = BoxLayout(orientation="vertical")

        title = Label(
            text="DUBBII DHOKATE\n\n"
                 "Hundee Namummaa fi Ijaarsa Keessoo\n\n"
                 "Barreessaa: Mudasir Mohammednur",
            font_size=24
        )

        chapters = Button(text="📚 Chapters")
        about = Button(text="👤 About Author")

        chapters.bind(
            on_press=lambda x: setattr(
                self.manager, "current", "chapters"
            )
        )

        about.bind(
            on_press=lambda x: setattr(
                self.manager, "current", "about"
            )
        )

        layout.add_widget(title)
        layout.add_widget(chapters)
        layout.add_widget(about)

        self.add_widget(layout)


class ChaptersScreen(Screen):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)

        layout = BoxLayout(orientation="vertical")

        chapters = [
            "Chapter 1: Of Beekuu",
            "Chapter 2: Humna Sammuu",
            "Chapter 3: Ijaarsa Keessoo",
            "Chapter 4: Kaayyoo Jireenyaa"
        ]

        for item in chapters:
            btn = Button(text=item)
            btn.bind(on_press=self.read)
            layout.add_widget(btn)

        back = Button(text="Deebi'i")
        back.bind(
            on_press=lambda x:
            setattr(self.manager,"current","home")
        )

        layout.add_widget(back)
        self.add_widget(layout)

    def read(self, button):
        self.manager.get_screen("reader").text.text = (
            button.text +
            "\n\n"
            "Barruun chapter kana as keessatti galma'a."
        )

        self.manager.current = "reader"


class ReaderScreen(Screen):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)

        layout = BoxLayout(orientation="vertical")

        self.text = Label(
            text="Chapter filadhu",
            font_size=18
        )

        back = Button(text="Deebi'i")
        back.bind(
            on_press=lambda x:
            setattr(self.manager,"current","chapters")
        )

        layout.add_widget(self.text)
        layout.add_widget(back)

        self.add_widget(layout)


class AboutScreen(Screen):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)

        self.add_widget(
            Label(
                text=
                "DUBBII DHOKATE\n\n"
                "Barreessaa: Mudasir Mohammednur"
            )
        )


class DubbiiDhokateApp(App):

    def build(self):

        sm = ScreenManager()

        sm.add_widget(HomeScreen(name="home"))
        sm.add_widget(ChaptersScreen(name="chapters"))
        sm.add_widget(ReaderScreen(name="reader"))
        sm.add_widget(AboutScreen(name="about"))

        return sm


DubbiiDhokateApp().run()
