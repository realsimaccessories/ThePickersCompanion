package au.com.ozswapntrade.m.thepickerscompanion;

import android.annotation.SuppressLint;
import android.content.Intent;
import android.graphics.Bitmap;
import android.graphics.drawable.Animatable;
import android.graphics.drawable.Drawable;
import android.net.MailTo;
import android.net.Uri;
import android.support.v4.widget.SwipeRefreshLayout;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.KeyEvent;
import android.view.View;
import android.webkit.WebSettings;
import android.webkit.WebView;
import android.webkit.WebViewClient;
import android.widget.ImageView;

public class MainActivity extends AppCompatActivity {

    private WebView mWebView;
    private SwipeRefreshLayout swipeContainer;

    private String urlPrefix;
    private static final String mailtoPrefix = "mailto";
    private static final String telPrefix = "tel";

    private static Intent newEmailIntent(String address, String subject, String body, String cc) {
        Intent intent = new Intent(Intent.ACTION_SEND);
        intent.putExtra(Intent.EXTRA_EMAIL, new String[]{address});
        intent.putExtra(Intent.EXTRA_TEXT, body);
        intent.putExtra(Intent.EXTRA_SUBJECT, subject);
        intent.putExtra(Intent.EXTRA_CC, cc);
        intent.setType("message/rfc822");
        return intent;
    }


    @SuppressLint("SetJavaScriptEnabled")
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        swipeContainer = (SwipeRefreshLayout) findViewById(R.id.swipeContainer);
        swipeContainer.setOnRefreshListener(new SwipeRefreshLayout.OnRefreshListener() {
            @Override
            public void onRefresh() {
                mWebView.reload();
                swipeContainer.setRefreshing(false);
                ImageView splashScreen = (ImageView) findViewById(R.id.splashScreen);
                Drawable drawable = splashScreen.getDrawable();
                ((Animatable) drawable).start();
                findViewById(R.id.splashScreen).setVisibility(View.VISIBLE);
                findViewById(R.id.solid).setVisibility(View.VISIBLE);
                findViewById(R.id.swipeContainer).setVisibility(View.GONE);
            }
        });

        mWebView = (WebView) findViewById(R.id.activity_main_webview);
        @SuppressWarnings("deprecation") WebViewClient mWebClient = new WebViewClient() {

            @Override
            public boolean shouldOverrideUrlLoading(WebView view, String url) {
                urlPrefix = url.split(":")[0];
                switch (urlPrefix) {
                    case mailtoPrefix:
                        MailTo mt = MailTo.parse(url);
                        Intent i = newEmailIntent(mt.getTo(), mt.getSubject(), mt.getBody(), mt.getCc());
                        startActivity(i);
                        view.reload();
                        return true;
                    case telPrefix:
                        Intent telIntent = new Intent(Intent.ACTION_DIAL, Uri.parse(url));
                        startActivity(telIntent);
                        return true;
                    default:
                        view.loadUrl(url);
                }
                return true;
            }

            @Override
            public void onPageStarted(WebView view, String url, Bitmap favicon) {
                super.onPageStarted(view, url, favicon);
                ImageView splashScreen = (ImageView) findViewById(R.id.splashScreen);
                Drawable drawable = splashScreen.getDrawable();
                ((Animatable) drawable).start();
                findViewById(R.id.swipeContainer).setVisibility(View.GONE);
                findViewById(R.id.solid).setVisibility(View.VISIBLE);
                findViewById(R.id.splashScreen).setVisibility(View.VISIBLE);
            }

            @Override
            public void onPageFinished(WebView mWebView, String url) {
                findViewById(R.id.splashScreen).setVisibility(View.GONE);
                findViewById(R.id.solid).setVisibility(View.GONE);
                findViewById(R.id.swipeContainer).setVisibility(View.VISIBLE);
            }
        };
        mWebView.setWebViewClient(mWebClient);
        WebSettings webSettings = mWebView.getSettings();
        webSettings.setJavaScriptEnabled(true);
        webSettings.setDomStorageEnabled(true);
        webSettings.setAllowContentAccess(true);
        webSettings.setAllowFileAccess(true);
        webSettings.setGeolocationEnabled(true);
        webSettings.setSupportZoom(true);
        webSettings.setBuiltInZoomControls(true);
        webSettings.setDisplayZoomControls(false);
        if (savedInstanceState == null) {
            mWebView.loadUrl("https://ozswapntrade.com.au");
        }
    }
    @Override
    public boolean onKeyDown ( int keyCode, KeyEvent event){
        if (event.getAction() == android.view.KeyEvent.ACTION_DOWN) {
            switch (keyCode) {
                case android.view.KeyEvent.KEYCODE_BACK:
                    if (mWebView.canGoBack()) {
                        mWebView.goBack();
                    } else {
                        finish();
                    }
                    return true;
            }

        }
        return super.onKeyDown(keyCode, event);
    }
    @Override
    protected void onSaveInstanceState (Bundle outState)
    {
        super.onSaveInstanceState(outState);
        mWebView.saveState(outState);
    }

    @Override
    protected void onRestoreInstanceState (Bundle savedInstanceState)
    {
        super.onRestoreInstanceState(savedInstanceState);
        mWebView.restoreState(savedInstanceState);
    }
}