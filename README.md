## Canal Oficial Flutter

- Iniciando com flutter, **_videos em inglês_** [Begin learning flutter](https://www.youtube.com/watch?v=4AoFA19gbLo&list=PLjxrf2q8roU3wk7CDw4RfV3mEwOJbjx1k).

**_Videos curtos muito bom para conhecer os packages e widgets do flutter._**
**_Possui legenda em portugues e outras linguas_**

- [Flutter Package of the week](https://www.youtube.com/watch?v=f9XQD5mf6FY&list=PLjxrf2q8roU1quF6ny8oFHJ2gBdrYN_AK) Playslist Recomendada para conhecer os diversos widgets que o Flutter tem a oferecer e aprender fazer o seu uso.

- [Flutter widget of the week](https://www.youtube.com/watch?v=f9XQD5mf6FY&list=PLjxrf2q8roU23XGwz3Km7sQZFTdB996iG) Playslist dos Packages favoritos

- Playlist com videos longos, mas com conhecimento avançado.
  [The boring Flutter Development Show](https://www.youtube.com/watch?v=Gz0x77Hd1GE&list=PLjxrf2q8roU3ahJVrSgAnPjzkpGmL9Czl) Desafios com os devs Google.

- [Flutter performance tips](https://www.youtube.com/watch?v=PKGguGUwSYE) dicas de performance.

## Outros Canais e Cursos

- Curso completo [Udemy](https://www.udemy.com/course/flutter-bootcamp-with-dart/?utm_source=adwords&utm_medium=udemyads&utm_campaign=Webindex_Catchall_la.EN_cc.BR&utm_term=_._ag_119370706961_._ad_488880694993_._kw__._de_c_._dm__._pl__._ti_dsa-406594358574_._li_1031545_._pd__._&matchtype=&gclid=CjwKCAiA3L6PBhBvEiwAINlJ9JYOFuq4E3ZAcFfOnwkfbvzxOPNWLEo_nKka69xIWhEeOH21o6jbLhoC7bYQAvD_BwE) Dr. Angela Yu - **_videos em inglês_**,

- Curso completo Official - inglês: [flutter-development-bootcamp-with-dart](https://www.appbrewery.co/p/flutter-development-bootcamp-with-dart)

- Curso - inglês: [Vandad Nahavandipoor](https://www.youtube.com/watch?v=IfUjHNODRoM&list=PL6yRaaP0WPkVtoeNIGqILtRAgd3h2CNpT), mais do que recomendavel.

- [Canal do Deivid Willyan](https://www.youtube.com/c/DeividWillyan) - Conteudo avançado e estrura de projeto, otimização, microapps, etc.

- [Canal Gabul Dev](https://www.youtube.com/c/GabulDEV) - Recomendado para quem está iniciando.

- [Curso-Flutter-Advanced](https://github.com/DeividWillyan/Curso-Flutter-Advanced) repositorio.

## Packages

- [Flutter Packages Favorites](https://pub.dev/flutter/favorites) - Packages favoritos no pub dev

## Documentacao
- [List of state management approaches](https://docs.flutter.dev/development/data-and-backend/state-mgmt/options) Gestão de estado.

### Tests
- [Testing](https://docs.flutter.dev/testing)
- [Testes de integração](https://docs.flutter.dev/testing/integration-tests)

- [Flutter Testing For Beginners - The Ultimate Guide](https://www.youtube.com/watch?v=RDY6UYh-nyg) guia para testes para quem está inciando no mundo FLutter.
### DART

- [Dart SDK](https://api.dart.dev/stable/2.15.1/index.html)

### CI/CD

- [Codemagic - flutterci](https://flutterci.com/)
- [GitHub - Exemplos Codemagic](https://github.com/codemagic-ci-cd/codemagic-sample-projects/tree/main/flutter)
- [Codemagic Docs](https://docs.codemagic.io/)
- [Github Actions](https://github.com/features/actions)
- [Docs Flutter](https://docs.flutter.dev/deployment/cd), como integrar com:
  - Codemagic
  - Bitrise
  - Appcircle
    - outros

## Design

- [RIVE](https://rive.app/) possibilita colocar assets animados e bonitos, sem custar o desempenho do device. Esses assests pode ser controlado dependendo de sua ação.
- [Rive Docs](https://help.rive.app/#getting-started) guia inical.

## Dicas para performace

```dart
/// não é recomendado
class MyWidget extends StatelessWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(child: myCustomWidget());
  }

  /// criando widget custom - dessa forma não é recomendado, a própria Google em um video explicando por qual motivo não fazer o uso, mas resumindo isso custa o uso de memoria do device e durante todo o build haverá um custo maior do que criar uma nova estrutura de widget.
  Widget myCustomWidget() => Container();

  /// evite adocionar funções na estrutura de um widget, isso implica em problemas em disparar ações sem intenção durante o build
  void myFunction() => print();

/// o recomendado
/// dessa forma o widget em si tem mais responsabilidade do seu uso
class MyWidget extends StatelessWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Container(child: MyCustomWidget());
  }
}

/// para resolução do uso indevido aqui um exemplo do nosso widget custom
class MyCustomWidget extends StatelessWidget {
  const MyCustomWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    // use sempre const quando possivel, isso fará muita diferença na performace
    return const SizedBox();
    }
  }
}
```
## Dicas para uso de modal ou snackbars
```dart
/// não é recomendado
/// criar funções quereceba como parametro contexto, a forma melhores de se trabalhar.
/// obs.: até pode se usar, mas resalva a exceções
/// exemplo:
class MyStore {
  void showSnackBar(BuildContext contextMain) {

    ScaffoldMessenger.of(contextMain).showSnackBar(
      const SnackBar(
        content: Text('A SnackBar has been shown.'),
      ),
    );
  }
}

class MyWidget extends StatelessWidget {
  const MyWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {

    return MyButton(onTap: () => MyStore().showSnackBar(context));
  }
}

/// recomendado tratar o contexto na estrura do widget
/// exemplo com uso de mixin
/// [sobre mixin](https://resocoder.com/2019/07/21/mixins-in-dart-understand-dart-flutter-fundamentals-tutorial/)

mixin MyModalMixin<T extends StatefulWidget> on State<T> {

  void showSnackBar({required String text}) {

    ScaffoldMessenger.of(context).showSnackBar(
      const SnackBar(
        content: Text(text),
      ),
    );
  }
}

class MyWidget extends StatefulWidget {
  MyWidget({Key? key}) : super(key: key);

  @override
  State<MyWidget> createState() => _MyWidgetState();
}

class _MyWidgetState extends State<MyWidget> with MyModalMixin {
  @override
  Widget build(BuildContext context) {
    return const MyButton(onTap: () => showSnackBar(text: 'A SnackBar has been shown.'));
  }
}

```
### Mais Dicas
- [Flutter tips and Tricks](https://github.com/vandadnp/flutter-tips-and-tricks) é um repositorio do @vandadnp, conteudo rico, vale apena conferir.
## Blogs, Medium, etc.
[Flutter](https://medium.com/flutter) Medium oficial do flutter.
[RESOCODER](https://resocoder.com/blog/) aqui temos diversos tutoriais, mas todos em inglês.
[Flutter Community](https://medium.com/flutter-community) Diversas postagens de pessoas envolvidas no mundo flutter (desempenho, design, boas praticas e muito mais)

