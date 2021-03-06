---
title: 'Tutorial: Azure Active Directory-Integration mit UserEcho | Microsoft Docs'
description: Erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und UserEcho konfigurieren.
services: active-directory
documentationcenter: 
author: jeevansd
manager: femila
editor: 
ms.assetid: bedd916b-8f69-4b50-9b8d-56f4ee3bd3ed
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/20/2017
ms.author: jeedes
translationtype: Human Translation
ms.sourcegitcommit: 2d8d925f80830a0d7047e9567fdd413af2e8c5c3
ms.openlocfilehash: c6ea5ae5f51ac5584840a18b2d995f52297ebcf9
ms.lasthandoff: 02/28/2017


---
# <a name="tutorial-azure-active-directory-integration-with-userecho"></a>Tutorial: Azure Active Directory-Integration mit UserEcho
Dieses Tutorial soll Ihnen zeigen, wie Sie UserEcho in Azure Active Directory (Azure AD) integrieren können.

Die Integration von UserEcho in Azure AD bietet die folgenden Vorteile: 

* Sie können in Azure AD steuern, wer auf UserEcho Zugriff hat. 
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei UserEcho anzumelden (einmaliges Anmelden, SSO).
* Sie können Ihre Konten an einem zentralen Ort verwalten – im klassischen Azure-Portal.

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit UserEcho konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Ein UserEcho-Abonnement, für das einmaliges Anmelden aktiviert ist

>[!NOTE]
>Um die Schritte in diesem Tutorial zu testen, wird empfohlen, keine Produktionsumgebung zu verwenden. 
> 

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie eine [einmonatige Testversion anfordern](https://azure.microsoft.com/pricing/free-trial/). 

## <a name="scenario-description"></a>Beschreibung des Szenarios
Ziel dieses Tutorials ist es, das einmalige Anmelden (SSO) von Azure AD in einer Testumgebung zu testen.  

Das in diesem Tutorial beschriebene Szenario besteht aus zwei Hauptelementen:

1. Hinzufügen von UserEcho aus dem Katalog 
2. Konfigurieren und Testen der einmaligen Anmeldung (SSO) von Azure AD

## <a name="add-userecho-from-the-gallery"></a>Hinzufügen von UserEcho aus dem Katalog
Zum Konfigurieren der Integration von UserEcho in Azure AD müssen Sie UserEcho aus dem Katalog der Liste der verwalteten SaaS-Apps hinzufügen.

**Um UserEcho aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld als Suchbegriff **UserEcho**ein.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_01.png)
7. Wählen Sie im Ergebnisbereich **UserEcho** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_02.png)

## <a name="configure-and-test-azure-ad-sso"></a>Konfigurieren und Testen des einmaligen Anmeldens (SSO) von Azure AD
In diesem Abschnitt soll anhand eines Testbenutzers namens Britta Simon veranschaulicht werden, wie das einmalige Anmelden von Azure AD in UserEcho konfiguriert und getestet werden kann.

Damit einmaliges Anmelden funktioniert, muss Azure AD wissen, welcher Benutzer in UserEcho als Entsprechung zu einem Benutzer in Azure AD fungiert. Anders ausgedrückt: Zwischen einem Azure AD-Benutzer und dem entsprechenden Benutzer in UserEcho muss eine Linkbeziehung eingerichtet werden.  

Diese Linkbeziehung wird hergestellt, indem Sie den Benutzernamen**** in Azure AD als Benutzernamen**** in UserEcho zuweisen.

Zum Konfigurieren und Testen des einmaligen Anmeldens von Azure AD bei UserEcho müssen Sie die folgenden Bausteine ausführen:

1. **[Konfigurieren des einmaligen Anmeldens von Azure AD](#configuring-azure-ad-single-single-sign-on)**, um Ihren Benutzern das Verwenden dieses Features zu ermöglichen.
2. **[Erstellen eines Azure AD-Testbenutzers](#creating-an-azure-ad-test-user)** – um das einmalige Anmelden mit Azure AD mit dem Testbenutzer Britta Simon zu testen.
3. **[Erstellen eines UserEcho-Testbenutzers](#creating-a-userecho-test-user)** , um eine Entsprechung von Britta Simon in UserEcho zu erhalten, die mit ihrer Darstellung in Azure AD verknüpft ist.
4. **[Zuweisen des Azure AD-Testbenutzers](#assigning-the-azure-ad-test-user)** – um Britta Simon für das einmalige Anmelden von Azure AD zu aktivieren.
5. **[Testen der einmaligen Anmeldung](#testing-single-sign-on)**, um zu überprüfen, ob die Konfiguration funktioniert.

### <a name="configure-azure-ad-sso"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
Das Ziel dieses Abschnitts besteht darin, das einmalige Anmelden (SSO) von Azure AD im klassischen Azure-Portal zu aktivieren und das einmalige Anmelden in Ihrer UserEcho-Anwendung zu konfigurieren. 

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD in UserEcho die folgenden Schritte aus:**

1. Klicken Sie im klassischen Azure-Portal auf der Anwendungsintegrationsseite für **UserEcho** auf **Einmaliges Anmelden konfigurieren**, um das Dialogfeld **Einmaliges Anmelden konfigurieren** zu öffnen.
   
    ![Einmaliges Anmelden konfigurieren][6] 
2. Wählen Sie auf der Seite **Wie sollen sich Benutzer bei UserEcho anmelden?** die Option **Azure AD – einmaliges Anmelden** aus, und klicken Sie dann auf **Weiter**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_03.png) 
3. Führen Sie auf der Dialogseite **App-Einstellungen konfigurieren** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_04.png) 
  1. Geben Sie im Textfeld **Anmelde-URL** die URL ein, die die Benutzer zur Anmeldung bei der UserEcho-Anwendung verwenden (z.B. *https://fabrikam.UserEcho.com/*).
  2. Klicken Sie auf **Weiter**.
4. Führen Sie auf der Seite **Einmaliges Anmelden konfigurieren für UserEcho** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_05.png) 
  1. Klicken Sie auf **Zertifikat herunterladen**und speichern Sie die Datei auf Ihrem Computer.
  2. Klicken Sie auf **Weiter**.
5. Melden Sie sich in einem anderen Webbrowserfenster bei der UserEcho-Unternehmenswebsite als Administrator an.
6. Klicken Sie auf der Symbolleiste oben auf Ihren Benutzernamen, um das Menü zu erweitern, und klicken Sie dann auf **Setup**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 
7. Klicken Sie auf **Integrations**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_07.png) 
8. Klicken Sie auf **Website** und dann auf **Single sign-on (SAML2)** (Einmaliges Anmelden (SAML2)).
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_08.png) 
9. Führen Sie auf der Seite **Single sign-on (SAML)** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_09.png) 
  1. Wählen Sie für **SAML-aktiviert** die Option **Ja**. 
  2. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite „Einmaliges Anmelden konfigurieren für UserEcho“ den Wert für **Dienst-URL für einmaliges Anmelden**, und fügen Sie ihn in das Textfeld **SAML-SSO-URL** ein.
  3. Kopieren Sie im klassischen Azure-Portal auf der Dialogfeldseite „Einmaliges Anmelden konfigurieren für UserEcho“ den Wert der **Remoteabmelde-URL**, und fügen Sie ihn in das Textfeld **Remote logout URL** (Remoteabmelde-URL) ein. 
  4. Öffnen Sie das heruntergeladene Zertifikat im Editor, kopieren Sie den Inhalt, und fügen Sie ihn anschließend in das Textfeld **X.509-Zertifikat** ein.    
  5. Klicken Sie auf **Speichern**.
10. Wählen Sie im klassischen Azure-Portal die Bestätigung zur Konfiguration des einmaligen Anmeldens aus, und klicken Sie dann auf **Weiter**. 
    
     ![Azure AD – einmaliges Anmelden][10]
11. Klicken Sie auf der Seite **Bestätigung zur einmaligen Anmeldung** auf **Fertig stellen**.  
    
     ![Azure AD – einmaliges Anmelden][11]

### <a name="create-an-azure-ad-test-user"></a>Erstellen eines Azure AD-Testbenutzers
In diesem Abschnitt wird im klassischen Azure-Portal eine Testbenutzerin namens Britta Simon erstellt.

![Azure AD-Benutzer erstellen][20]

**Um einen Testbenutzer in Azure AD zu erstellen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/create_aaduser_09.png)  
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Anzeigen der Liste der Benutzer im Menü oben auf **Benutzer**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/create_aaduser_03.png) 
4. Um das Dialogfeld **Benutzer hinzufügen** zu öffnen, klicken Sie auf der Symbolleiste unten auf **Benutzer hinzufügen**. 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/create_aaduser_04.png) 
5. Führen Sie auf der Dialogfeldseite **Informationen über diesen Benutzer** die folgenden Schritte aus: 
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/create_aaduser_05.png)  
  1. Wählen Sie als „Benutzertyp“ die Option „Neuer Benutzer in Ihrer Organisation“ aus.
  2. Geben Sie in das Textfeld **Benutzername** den Namen **BrittaSimon** ein.
  3. Klicken Sie auf **Weiter**.
6. Führen Sie auf der Dialogfeldseite **Benutzerprofil** die folgenden Schritte aus: 
   
   ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/create_aaduser_06.png) 
  1. Geben Sie in das Textfeld **Vorname** den Namen **Britta** ein.  
  2. Geben Sie in das Textfeld **Nachname** den Namen **Simon** ein.
  3. Geben Sie in das Textfeld **Anzeigename** den Namen **Britta Simon** ein.
  4. Wählen Sie in der Liste **Rolle** die Option **Benutzer** aus.
  5. Klicken Sie auf **Weiter**.
7. Klicken Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** auf **Erstellen**.
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/create_aaduser_07.png) 
8. Führen Sie auf der Dialogfeldseite **Vorübergehendes Kennwort abrufen** die folgenden Schritte aus:
   
    ![Erstellen eines Azure AD-Testbenutzers](./media/active-directory-saas-userecho-tutorial/create_aaduser_08.png) 
  1. Notieren Sie den Wert von **Neues Kennwort**.
  2. Klicken Sie auf **Fertig stellen**.   

### <a name="create-a-userecho-test-user"></a>Erstellen eines UserEcho-Testbenutzers
Das Ziel dieses Abschnitts ist das Erstellen eines Benutzers namens Britta Simon in UserEcho.

**Um einen Benutzer namens Britta Simon in UserEcho zu erstellen, führen Sie die folgenden Schritte aus:**

1. Melden Sie sich bei der UserEcho-Unternehmenswebsite als Administrator an.
2. Klicken Sie auf der Symbolleiste oben auf Ihren Benutzernamen, um das Menü zu erweitern, und klicken Sie dann auf **Setup**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_06.png) 
3. Klicken Sie auf **Benutzer**, um den Abschnitt **Benutzer** zu erweitern.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_10.png) 
4. Klicken Sie auf **Users**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_11.png) 
5. Klicken Sie auf **Invite a new user**.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_12.png)
6. Führen Sie im Dialogfeld **Invite a new user** die folgenden Schritte aus:
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_13.png) 
  1. Geben Sie in das Textfeld **Name** den Namen **Britta Simon** ein.
  2. Geben Sie im Textfeld **E-Mail** die E-Mail-Adresse von Britta Simon im klassischen Azure-Portal ein.
  3. Klicken Sie auf **Invite**.

Eine Einladung wird an Britta gesendet, die ihr Zugriff auf UserEcho gewährt. 

### <a name="assign-the-azure-ad-test-user"></a>Zuweisen des Azure AD-Testbenutzers
Das Ziel dieses Abschnitts besteht darin, Britta Simon die Verwendung des einmaligen Anmeldens (SSO) von Azure zu ermöglichen, indem sie Zugriff auf UserEcho erhält.

![Benutzer zuweisen][200] 

**Um Britta Simon UserEcho zuzuweisen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie zum Öffnen der Anwendungsansicht im klassischen Azure-Portal in der Verzeichnisansicht im oberen Menü auf **Anwendungen** .
   
    ![Benutzer zuweisen][201] 
2. Wählen Sie in der Anwendungsliste **UserEcho**aus.
   
    ![Einmaliges Anmelden konfigurieren](./media/active-directory-saas-userecho-tutorial/tutorial_userecho_50.png) 
3. Klicken Sie im oberen Menü auf **Benutzer**.
   
    ![Benutzer zuweisen][203] 
4. Wählen Sie in der Benutzerliste **Britta Simon**aus.
5. Klicken Sie auf der Symbolleiste unten auf **Zuweisen**.
   
    ![Benutzer zuweisen][205]

### <a name="test-single-sign-on"></a>Testen des einmaligen Anmeldens
Das Ziel dieses Abschnitts ist das Testen Ihrer Azure AD-Konfiguration für einmaliges Anmelden (SSO) über den Zugriffsbereich.  

Wenn Sie im Zugriffsbereich auf die Kachel „UserEcho“ klicken, sollten Sie automatisch in Ihrer UserEcho-Anwendung angemeldet werden.

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->

[1]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_04.png

[6]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_05.png
[10]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_06.png
[11]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_07.png
[20]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_100.png

[200]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_200.png
[201]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_201.png
[203]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_203.png
[204]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_204.png
[205]: ./media/active-directory-saas-userecho-tutorial/tutorial_general_205.png







