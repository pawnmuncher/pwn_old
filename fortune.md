<!DOCTYPE html>
<html lang="en">

<head>
  <title>Fortune Teller V1</title>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">

  <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.2.1/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
  <style>
    body {
      margin: 0 auto;
      padding: 0 auto;
      width: 50%;
    }

    input[type="checkbox"] {
      margin: 1.0rem;
    }

    #goodFortune {
      min-height: 2.0rem;
      padding: 15px;
      margin: 15px;
      display: none;
      background-color: lightgreen;
    }

    #badFortune {
      min-height: 2.0rem;
      padding: 15px;
      margin: 15px;
      display: none;
      background-color: lightcoral;
    }

    #noFortune {
      min-height: 2.0rem;
      padding: 15px;
      margin: 15px;
      display: none;
      background-color: lightgray;
    }
  </style>
</head>

<body>
  <div class="container-fluid jumbotron">
    <h1 class="display-1">Fortune Teller V1</h1>
  </div>
  <form id="fortune_survey">
    <div class="form-group" id="fortune-survey">
      <p>Please check the skills you have for the job you wish to apply.
        <strong>
          <em>Hey there: in the past week did you save the planet?</em>
        </strong>:
      </p>
      <label for="name">Enter your name please</label>
      <input type="text" required name="name" id="name" class="form-control" placeholder="J Doe">
      <br>
      <input type="checkbox" name="lucky" value="winner">You are a winner!
      <br>
      <input type="checkbox" name="unlucky" value="cat">Saw a black cat.
      <br>
      <input type="checkbox" name="lucky" value="clover">Saw a Clover anywhere.
      <br>
      <input type="checkbox" name="unlucky" value="ladder">Walked under a ladder.
      <br>
      <input type="checkbox" name="lucky" value="penny">Found a penny.
      <br>
      <input type="checkbox" name="unlucky" value="mirror">Broke a mirror.
      <br>
      <input type="checkbox" name="lucky" value="prayer">Had a prayer answered.
      <br>
      <br>
      <button type="submit" class="btn btn-primary btn-block">Next</button>
  </form>
  </div>

  <span id="fortune_result">
    <p id="badFortune">
      <span class="name"></span>, so sorry but will spend the next day agonizing over your past indiscretions against
      your loved ones.
      <br>
      <br>
      <br>
      <input class="btn btn-danger" type="button" onclick="location.reload();" value="Try Again?">
    </p>

    <p id="goodFortune">
      <span class="name"></span>, there is nothing but wealth and love heading your way! Rejoice in your great fortune!
      <br>
      <br>
      <br>
      <input class="btn btn-danger" type="button" onclick="location.reload();" value="Try Again?">
    </p>
    <p id="noFortune">
      <span class="name"></span>, you have no fortune! Your life will forever be unchanged.
      <br>
      <br>
      <br>
      <input class="btn btn-danger" type="button" onclick="location.reload();" value="Try Again?">
    </p>
  </span>
  <br>
  <script>
    //need to add css and jquery to hide second set of questions until first submit, then display second set and on submit display both results. Add submit button 2 as well
    $(document).ready(function () {
      $("form#fortune_survey").submit(function (event) {
        event.preventDefault();
        var unluckyArray = [];
        var luckyArray = [];
        $("input:checkbox[name=unlucky]:checked").each(function () {
          var unlucky = $(this).val();
          unluckyArray.push(unlucky);
          return unluckyArray;
        });
        $("input:checkbox[name=lucky]:checked").each(function () {
          var lucky = $(this).val();
          luckyArray.push(lucky);
          return luckyArray;
        });
        $("#fortune_survey").hide();

        if (unluckyArray.length > luckyArray.length) {
          $("#badFortune").show();
        } else if (luckyArray.length > unluckyArray.length) {
          $("#goodFortune").show();
        } else {
          $("#noFortune").show();
        }
        var name = $("#name").val();
        $(".name").text(name);
        $("#fortune").hide();
        event.preventDefault();
      });
    });
  </script>
</body>

</html>
